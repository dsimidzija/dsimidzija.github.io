---
layout: post
title: "Speeding up pytest + sqlalchemy"
description: ""
category: Programming
tags: [programming, python, unit-tests, sqlalchemy]
---
{% include JB/setup %}

A large chunk of python projects use SQLAlchemy as ORM, and in doing so may
actually be doubling their test runs.

<a name="excerpt-continue"></a>

One of the more interesting things I discovered while running tests with
[profiling][pytest-profiling] on is the fact that the most expensive operations
were actually involving database fixtures and user auth. This didn't make any
sense at first, because it was supposed to be a very cheap set of operations on
a very small scale.

However, on closer inspection I realised that this wasn't
[SQLAlchemy][sqlalchemy] per se, it was actually [bcrypt][bcrypt]!
As it is a decently written cryptography library, bcrypt operations are
somewhat expensive (although I'm not sure at this point that it is not
vulnerable to [timing attacks][timing-attack]?), and in the context of unit
tests, it turns out that they were the _most_ expensive operations.

What this means in the practical sense, is that all of the
`sqlalchemy_utils.PasswordType` columns will spend a significant amount of time
when doing almost anything useful (inserts, updates, comparisons). Since we
don't really care about password security during tests, this is simply a waste
of time, and a waste of CPU cycles in your pipeline.

We solve this problem by doing some [metaprogramming][metaclasses], and
replacing the `PasswordType` with a plain old `String` column. The rest of your
code should be none the wiser.

```python
from flask_sqlalchemy import SQLAlchemy
from flask_sqlalchemy.model import DefaultMeta
from sqlalchemy.ext.declarative import as_declarative
from sqlalchemy_utils import PasswordType

from yourproject.config import ActiveConfig

db = SQLAlchemy()


class BaseModelMetaclass(DefaultMeta):
    def __new__(cls, name, bases, dct):
        obj = super().__new__(cls, name, bases, dct)

        if ActiveConfig.TESTING:
            for key, value in dct.items():
                if not isinstance(value, db.Column):
                    continue
                if isinstance(value.type, PasswordType):
                    value.type = db.String(100)

        return obj


@as_declarative(
    metaclass=BaseModelMetaclass,
)
class BaseModel(db.Model):
    __abstract__ = True
```

This example uses `flask-sqlalchemy`, but obviously it will work without the
flask wrappers. The key element here is the `ActiveConfig.TESTING` flag, so you
don't accidentally replace all the password fields in production as well.

Results may vary of course, but for the first project where we applied this,
test runs went from ~14.5 minutes to ~6 for less than a 1000 tests. For large
projects, time savings could be even more significant.

[pytest-profiling]: https://pypi.org/project/pytest-profiling/
[sqlalchemy]: https://www.sqlalchemy.org/
[bcrypt]: https://github.com/pyca/bcrypt/
[timing-attack]: https://en.wikipedia.org/wiki/Timing_attack
[metaclasses]: https://docs.python.org/3/reference/datamodel.html#metaclasses
