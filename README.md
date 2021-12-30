# What

This is the source of my homepage <https://dsimidzija.github.io/>.

## Using

* [Jekyll][]
* [Chirpy][]

`rvm` / `bundler` / `rake`

At the time of writing, running with `ruby-2.5.1`.

### Running locally

```bash
$ jekyll serve --incremental --livereload
```

## Misc troubleshooting

### No bundler exe found

If you get:

    Can't find gem bundler (>= 0.a) with executable bundle (Gem::GemNotFoundException)

Do this:

    $ cat Gemfile.lock | grep -A 1 "BUNDLED WITH"
    BUNDLED WITH
       2.1.4

    $ gem install bundler -v '2.1.4'

[Jekyll]: https://jekyllrb.com/
[Chirpy]: https://github.com/cotes2020/jekyll-theme-chirpy
