---
layout: post
title: "A proper way of learning Vim"
description: ""
category: Programming
tags: [vim, tricks, productivity, beginner]
---

When I started learning [The Editor][vimorg] years ago, I tried out different things to see what is the best way of doing it. Here are some tips which may helpful to you if you're struggling.
<a name="excerpt-continue"></a>

We won't go into whether it's worth learning how to use Vim. [It][1] [simply][2] [is][3].

The hardest part about learning Vim is probably the fact that it's just so different from anything you've encountered before (and it truly is). So the [learning curve][] is quite steep, since you probably won't even be able to quit when you first get in. I know I didn't..

The core of the problem is that Vim has a huge amount of commands, even when it's not extended by anything. Trying to commit a lot of commands to memory when you've just started using it is a recipe for a disaster. Unless you have exceptional cerebral powers, there is no way you're going to remember anything more than a few most frequently used commands. Within this simple fact lies a trick for learning Vim properly.

The trick is: __learn slowly__. In order to use Vim properly and _fast_, you need to build up some muscle memory first. When you start using it, just moving around with `hjkl` will be amazingly unintuitive and slow. So don't think about all the other commands just yet. Use your first week or two to just use Vim like a jackass: use `hjkl` to move around, `i` to go to insert mode and `ESC` to go back to normal mode. That's it.

When you start getting bored with that, remember that you can spice things up with numbers: `5j`, `10h` etc.. You can also go to insert mode and skip to the beginning of the line with `I` (capital `i`), or go to the end of the line with `$`, move around with `w`/`W`/`b`/`B`. Ten words? No problem, `10w`!

When you've started navigating with your muscle memory, the real fun begins. You're now ready to introduce more commands, but only a few at a time (maybe even just one!). The key thing to remember is that every command you learn expands the power of Vim almost exponentially. Why? Because commands can _and should_ be combined!

For example, if you know that `10j` will move your caret 10 lines below, when you learn that `d` means delete, you can now do `d10j` to delete 10 lines below your current position (technically, you'll delete 11 lines, your current line and 10 below it). Or `d10w` to delete 10 (i.e. 11) words. Or `d$` to delete everything until the end of the line. Now think what you can do when you combine only the commands you can find on any [Vim cheatsheet][sheet] online.

The concept of combining Vim commands is precisely what makes Vim _The Editor_, and it is best described here: [What is your most productive shortcut with Vim?](http://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim). When you've figured that out, the rest will come naturally. Whenever you need to do something in Vim and it seems slow, there's probably a more efficient way of doing it. You just need to find it.

Just don't rush it!:wq

[1]: http://www.terminally-incoherent.com/blog/2012/03/21/why-vim/
[2]: http://usevim.com/2012/10/26/why-vim/
[3]: http://stackoverflow.com/questions/597077/is-learning-vim-worth-the-effort
[vimorg]: http://www.vim.org/
[learning curve]: http://blog.lookthink.com/wp-content/uploads/2014/01/text_editors.jpg
[sheet]: https://vim.rtorr.com/
