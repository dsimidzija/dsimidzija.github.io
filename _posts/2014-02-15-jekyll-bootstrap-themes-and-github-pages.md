---
layout: post
title: "Jekyll-Bootstrap themes and GitHub Pages"
description: ""
category: Programming
tags: [jekyll, jekyll-bootstrap, github, github-pages, fix]
---
{% include JB/setup %}

After setting up jekyll-bootstrap and playing with it for a while, I decided to
try and set up a custom theme by following the [official instructions][jb-theming].
Several themes are conveniently available at [themes.jekyllbootstrap.com][jb-themes],
including the theme you're looking at right now. 

After running `rake theme:install git="https://github.com/dhulihan/hooligan.git"`
and switching to the newly installed theme, I tried to push to GitHub. Unfortunately,
five minutes later, I received the following email:

```
The page build failed with the following error:

The submodule `_theme_packages/hooligan` was not properly initialized with a `.gitmodules` file.

For information on troubleshooting Jekyll see:

  https://help.github.com/articles/using-jekyll-with-pages#troubleshooting
```

Of course, looking at the troubleshooting page didn't help. The issue seemed to be
that the above command adds a new git submodule to `_theme_packages/hooligan` but
does not create the `.gitmodules` file. The solution was simple, although
annoyingly undocumented: add the `_theme_packages` folder to `.gitignore`.

Since the folder in question was already in the repo, I had to remove it:

```bash
git rm -r --cached _theme_packages
```

Then simply edit your `.gitignore` and add the `_theme_packages` folder:

```
*.swp
_site
_theme_packages
 
```

Finally, commit and push to GitHub and you should be done. I'm not sure whether
this is the correct way to resolve the issue, it looks like this may be a bug
in jekyll-bootstrap.

[jb-theming]: http://jekyllbootstrap.com/usage/jekyll-theming.html
[jb-themes]: http://themes.jekyllbootstrap.com/

