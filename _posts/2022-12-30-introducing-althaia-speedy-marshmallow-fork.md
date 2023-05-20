---
layout: post
title: "Introducing Althaia: a speedy Marshmallow fork"
description: "Althaia is a drop-in replacement for the #1 Python serialization library - marshmallow"
category: Programming
tags: [programming, python, marshmallow, serialization, marshalling, althaia]
pin: true
---
_Althaia_ is a drop-in replacement for the #1 Python serialization library -
[marshmallow][]. It is also a shallow fork of marshmallow, basically using
the entire marshmallow source with a few patches to speed up the `dump` operations.

[![GitHub](https://badgen.net/badge/icon/github?icon=github&label)][althaia-repo]
[![PyPI Version](https://badgen.net/pypi/v/althaia)][althaia-pypi]
[![License](https://badgen.net/pypi/license/althaia)][althaia-pypi]
[![PyPI Python](https://badgen.net/pypi/python/althaia)][althaia-pypi]
{: style="text-align: center" }

## TL;DR

```bash
$ pip install althaia
```

```python
# patch python's sys.meta_path to allow althaia to handle all marshmallow imports
import althaia
althaia.patch()
```
{: file="some/early/bootstrap/__init__.py" }

```python
# use marshmallow as if nothing changed, except now it's 45% faster
from marshmallow import Schema


class ConquestOfBread(Schema):
    # etc...
```
{: file="everywhere/else/inyourproject.py" }

## Background

A while back at work, we ran into some performance issues with a few endpoints while dumping
some _mildly_ large datasets. This was quite surprising because these were just regular
JSON objects with fairly common fields. Additionally, we're not talking about _millions_
or even _billions_ of rows, just somewhere in the range of 5k-20k query results (so far).

Upon further inspection, we realised the problem was marshmallow, and not our own code.
This is a surprise, to be sure, but an unwelcome one. Sadly, there was no time to do anything
about it as the project had to be done on schedule, so we simply worked around the issue
as best as we could.

However, this continued to bug me privately - why is it so inefficient? While attempting to
work around this performance problem, I attempted to find an alternative serialization
library, one that could easily replace marshmallow. One of the first results you get when
you try to find something like that is the [Python Serialization Benchmark][benchmarks] page,
which lists most relevant serialisation libraries, and some lesser-known ones.

While going through the list, I realised that all the alternatives lack some of the key
features marshmallow offers, most notably [dynamic schemas][], but also the entire ecosystem
of validators and helper libraries that surrounds marshmallow. Not to mention the fact that
we had a somewhat mature project on our hands, and reimplementing everything we did with
marshmallow with some of the more obscure libraries is a rather dubious endeavor resource-wise,
even if we had proof that it can be done.

There was no other way to find out what's happening than to fire up [cProfile][], more
specifically [line_profiler][], and try to figure out why marshmallow was underperforming.
It didn't take long to figure out that the culprits were `Schema.get_attribute()` and
`Field.serialize()`.

## The Problem

These two methods are at the core of marshmallow's dumping operations, and their fatal flaw
is doing what is considered a semi-joke definition of madness: doing the same operation over
and over again, and expecting a different outcome. In other words, for each object in a
collection that is being dumped, marshmallow is trying to get the field _configuration_, in
addition to the field _value_.

At first glance, this is a reasonable thing to do; for example, you have to figure out how
to behave when a field is missing in the source object. The problem arises when you're dumping
somewhat large collections of objects, where marshmallow is going to try to detect how to
behave _for each field and each object in the collection_. If you're dumping a collection of
10k objects, and a single object has 15 fields, that's 150k(!) lookups. In addition, if you
take into consideration that each lookup involves multiple deep-level calls all around the
inheritance tree, things get very slow _very quickly_ (pun intended obviously).

## The Solution(s)

So I wanted to (try to) do two things:

* See if we can somehow figure out some things only once while dumping a collection
* Compile everything with Cython and see if it makes any difference

As it turns out, both things were possible, and they bring about a significant performance
improvement! The initial version of "first item" fixes were submitted to marshmallow repo as
[PR #1649][pr1649], where I also discovered that this problem was already discussed in detail
in [issue #805][issue805].

Additionally, to compile marshmallow with Cython, I needed to change some of the type hints.
What's weird about those is that `type` and `typing.Type[type]` are supposed to be equivalent,
but apparently the compiler doesn't think so.

## How does it perform?

According to the "official" tests from the marshmallow repo, the two fixes above shave off about
45% of execution time (~35% coming for the code changes, ~10% coming from the Cython compilation).
For large(-ish) datasets, this is quite significant.

Compared to [Toasted Marshmallow][toasted], Althaia seems to perform slightly worse, but I
still need to run proper benchmarks and contribute a PR to the Serialization Benchmarks repo
(the initial run is already visible in the [README.md][readme-how-fast]). The problem with _Toasted_
is that it requires client-side code changes to use, and the fact that it is stuck on using
marshmallow v2, which is quite old right now and already outperformed by marshmallow v3+.
Not to mention that it [doesn't support dotted syntax][toasted-dotted] and other goodies.
Althaia maintains full compatibility with marshmallow, with nearly the same gains as when using
_Toasted_.

**Note:** The benchmarks in _Python Serialization Benchmark_ are a bit fishy: it looks like
_Toasted Marshmallow_ is [overwriting the marshmallow installation][toasted-benchmark], so the
results for it are severely misrepresented by testing the version installed by _Toasted_.
Let's compare the numbers (all values are usec/dump from the official benchmark):

| Toasted (base/v2) | Toasted (optimized) | Latest Marshmallow | Althaia |
|------------------:|--------------------:|-------------------:|--------:|
|           2682.61 |              176.38 |             444.91 |  239.24 |

The first two numbers are taken from the _Toasted_ readme, while the last two are local benchmarks.
Obviously, _Toasted_ base marshmallow is _very_ slow compared to the latest release. The entire
repo may require a rewrite to compare each library in a separate environment.

Surprisingly (but also maybe not), running Althaia on PyPy with the cython-compiled modules actually
_reduces_ performance by a very significant amount, so the compilation step has been disabled for
PyPy installations. The code optimisations improve the performance on their own by about 15%
(79.85 vs 68.38 usec/dump), quite a bit less than the average 45% gain on CPython.

## How does it work?

The final implementation is actually fairly simple, we use the upstream marshmallow as a git
submodule, copy all the sources and change all the namespaces from `marshmallow` to
`althaia.marshmallow`, and then apply our patches (from the `patches/` folder). Then we compile
everything with Cython, and build all the various binary wheels. Source install will require
cython to compile the modules locally, and while it's not the slowest thing in the world, you
will notice it. Contributions for more pre-built wheels are welcome.

## Closing notes

I've never built or maintained a python package, let alone a cython variant. So there were a
few gotchas worth noting:

* Publishing packages on PyPI is a potential minefield if you're not careful - once you publish
  a package, it cannot be updated, replaced, or otherwise modified. Clearly, that makes sense for
  many reasons (security being a major factor), but it may not be obvious to a new user. Luckily,
  there is <https://test.pypi.org> where you can experiment before doing the real thing.

* Creating a non-trivial Python package is _convoluted_. Somewhat replicating the experience of
  [this reddit rant][reddit-rant], when you start looking into it, you can get easily distracted:
  setup.py, setup.cfg, distutils, setuptools, cython, cffi, PyPA build, ...

  [![relevant xkcd](https://imgs.xkcd.com/comics/python_environment.png)](https://xkcd.com/1987/)
  _Relevant xkcd_
  {: style="text-align: center" }

  Luckily, things are greatly simplified by using `pyproject.toml`, [PEP 517][pep-517], and
  [Poetry][poetry] - probably the nicest tool you'll find in this whole mess.

* Building [manylinux][] (et al.) wheels is also a tricky task on its own, but luckily there is
  [cibuildwheel][] which helps _a lot_. However, even that doesn't solve everything if you have
  a custom build/test scripts, like Althaia has, which is painfully obvious in the
  `[tool.cibuildwheel]` section(s) of `pyproject.toml`.

* If you plan on using the previously mentioned [PEP 517][pep-517] frontend for your custom builds,
  _do not name your build script_ `build.py`, or suddenly you won't be able to actually produce
  a wheel. The reason is, of course, that `python -m build` is now running _your_ `build.py`, and
  not the [PyPA build][pypa-build] frontend.

* Attempting to compile `__init__.py` with cython on Windows will get you
  [some weird looks from the linker][cython-linker-issue] due to ~~_some bullshit_~~ [historical
  reasons][python-issue35893]. The issue is marked as fixed, but it's not fixed in the cython
  version published on PyPI.

* If you have a slow task which is CPU-bound, good luck trying to do it concurrently under CPython,
  and working around the [Global Interpreter Lock][python-gil].

## Links

* [Althaia @ PyPI][althaia-pypi]
* [Althaia @ GitHub][althaia-repo]
* [Marshmallow Docs][marshmallow]
* [Marshmallow @ GitHub][marshmallow-repo]


[althaia-pypi]: https://pypi.org/project/althaia/
[althaia-repo]: https://github.com/dsimidzija/python-althaia
[benchmarks]: https://voidfiles.github.io/python-serialization-benchmark/
[cProfile]: https://docs.python.org/3/library/profile.html
[cibuildwheel]: https://github.com/pypa/cibuildwheel
[cython-linker-issue]: https://github.com/cython/cython/issues/2968
[dynamic schemas]: https://stevenloria.com/dynamic-schemas-in-marshmallow/
[issue805]: https://github.com/marshmallow-code/marshmallow/issues/805
[line_profiler]: https://github.com/rkern/line_profiler
[manylinux]: https://github.com/pypa/manylinux
[marshmallow-repo]: https://github.com/marshmallow-code/marshmallow
[marshmallow]: https://marshmallow.readthedocs.io/
[pep-517]: https://www.python.org/dev/peps/pep-0517/
[poetry]: https://python-poetry.org/
[pr1649]: https://github.com/marshmallow-code/marshmallow/pull/1649
[pypa-build]: https://github.com/pypa/build
[python-gil]: https://wiki.python.org/moin/GlobalInterpreterLock
[python-issue35893]: https://bugs.python.org/issue35893
[readme-how-fast]: https://github.com/dsimidzija/python-althaia/blob/feature/ci/README.md#how-fast-is-it
[reddit-rant]: https://www.reddit.com/r/Python/comments/fd039n/python_packaging_documentation_sucks/
[toasted-benchmark]: https://github.com/voidfiles/python-serialization-benchmark/issues/12
[toasted-dotted]: https://github.com/marshmallow-code/marshmallow/issues/805#issuecomment-530954571
[toasted]: https://github.com/lyft/toasted-marshmallow
