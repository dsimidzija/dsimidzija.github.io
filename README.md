# What

This is the source of my homepage <https://dsimidzija.github.io/>.

## Using

* [Jekyll][]
* [Chirpy][]

`rvm` / `bundler` / `rake`

At the time of writing, running with `ruby-3.4.*`.

### Running locally

```bash
$ jekyll serve --incremental --livereload
```

### Updating

```bash
bundle update
```

If `jekyll-theme-chirpy` is updated, also update the assets:

```bash
cd assets/lib
git pull --prune
cd -
```

## Misc troubleshooting

### No bundler exe found

If you get:

    Can't find gem bundler (>= 0.a) with executable bundle (Gem::GemNotFoundException)

Do this:

```bash
$ cat Gemfile.lock | grep -A 1 "BUNDLED WITH"
BUNDLED WITH
   2.1.4

$ gem install bundler -v '2.1.4'
```

### Jekyll kernel load errors

This happens with `google-protobuf` and `ffi` gems for some reason. Example error:

```bash
...
protobuf_native.rb:15:in 'Kernel#require': cannot load such file -- google/protobuf_c
...
```

Solution:

```bash
gem uninstall PACKAGE_NAME
gem install --platform ruby PACKAGE_NAME
```

If `gem uninstall PACKAGE_NAME` is returning multiple versions of the same package, and one of them ends with
`-x86_64-linux`, it should be enough to just remove that.

[Jekyll]: https://jekyllrb.com/
[Chirpy]: https://github.com/cotes2020/jekyll-theme-chirpy
