---
layout: post
title: Learning Vim in a Week
date: 2014-02-08 13:22:12.000000000 -08:00
---
**Note:** I turned this blog post into a talk for Boston Vim, check it out here: [Learning Vim in a Week - Boston Vim](https://mikecoutermarsh.com/boston-vim-learning-vim-in-a-week/).

---
I'd been using Sublime for a long time and recently switched over to Vim. It look me about a full week to learn enough to use it daily for work. Here is what I did to do it!


###Before you start, you should know...
* All that stuff Sublime or TextMate can do. So can Vim. Plus more.
* You'll be able to keep your entire workflow in a single terminal window, without taking your hands off the keyboard.
* __.vimrc.__ This is the file you modify to customize Vim, you'll become very familiar with it.
* Don’t start learning at work, it’s frustrating at first and you won’t be able to get much done. Weekends are good.
### Getting started...
These are roughly the steps I followed to learn Vim.

* Grab a coffee, open a terminal, and run the command <em>vimtutor</em>. Go through it a couple times (I did it sporadically over a couple days).
* There’s also a gamified version of vimtutor, <a href="http://vim-adventures.com/" target="_blank" rel="nofollow">Vim Adventures</a>.
* Steal someone else's <em>.vimrc</em> and <em>.vimrc.bundles</em>. These files let you add plugins and customize Vim. <a href="https://github.com/thoughtbot/dotfiles" target="_blank" rel="nofollow">Thoughtbot’s is good</a>. Here’s <a href="https://github.com/mscoutermarsh/dotfiles" target="_blank" rel="nofollow">mine</a>. You'll eventually have your own, but it helps to start with someone elses.
* <a href="http://stackoverflow.com/questions/127591/using-caps-lock-as-esc-in-mac-os-x" target="_blank" rel="nofollow">Remap your Caps Lock to ESC</a>. You will use ESC in Vim _constantly_. Having it closer to your home row helps.
* Speed up your key repeat rate. This is how quickly a key press repeats when you hold down a key. It helps you scroll through code much faster. On OSX you can do this <a href="https://pqrs.org/macosx/keyremap4macbook/" target="_blank" rel="nofollow">KeyRemap4MacBook</a>.
* Watch screencasts. Thoughtbot <a href="https://learn.thoughtbot.com/vim" target="_blank" rel="nofollow">has a ton of great ones if you subscribe to prime</a>. The one I found most helpful was “<a href="https://learn.thoughtbot.com/purchases/8889fb98cf0046fb0ae61a1597584573" target="_blank" rel="nofollow">Vim for Rails Developers</a>.”

### Switching Files and Searching:
The biggest barrier for me using Vim full time was quickly navigating a project. There are bundles you can use to make this really easy.
<h3><a href="https://github.com/kien/ctrlp.vim" target="_blank" rel="nofollow">CTRLP</a></h3>
This does the same thing as Sublime’s Ctrl P. Fuzzy search by file name. Must have.

![vim ctrl p](/assets/archive/images/2014/Oct/ctrlp.jpg)
<h3><a href="https://github.com/scrooloose/nerdtree" target="_blank" rel="nofollow">NERDTree</a></h3>

Gives you a sidebar that you can quickly navigate files with. I’ve mapped NERDTree to &lt;F10&gt; so I can quickly open and close it. I've also mapped &lt;F9&gt; to bring me to my currently open file in NERDTree (super useful).

```vim
" Toggle nerdtree with F10
map <F10> :NERDTreeToggle<CR>

" Current file in nerdtree
map <F9> :NERDTreeFind<CR>
```

![nerdtree](/assets/archive/images/2014/Oct/nerdtree.jpg)
### <a href="https://github.com/rking/ag.vim" target="_blank" rel="nofollow">Ag for Vim</a> (Project wide search)
Super fast search. Also speeds up indexing when using CtrlP.

![ag](/assets/archive/images/2014/Oct/ag.jpg)
<h3></h3>
<h3>Copy &amp; Paste:</h3>
I struggled for a while with copying and pasting code from outside Vim. It’s just a little different than other text editors and this makes the transition a little rough.

I recommend reading this <a href="http://vim.wikia.com/wiki/Cut/copy_and_paste_using_visual_selection" target="_blank" rel="nofollow">article on copy/paste</a>.

Also, if you have Vim’s auto indent and auto commenting turned on (which you probably do and want). When you paste, code may automatically be reformatted or commented out. This is annoying.

Vim has a “paste mode” that you can toggle on and off. I mapped it to &lt;F2&gt; so that when I do need to paste from outside of Vim, I can tap a key to do it.

```vim
"key to insert mode with paste using F2 key
map <F2> :set paste<CR>i
" Leave paste mode on exit
au InsertLeave * set nopaste
```

<h2>Finally, Commit to it</h2>
Once I had the basics down, I hid Sublime from my dock and started using Vim for real work. Anytime I found I was slow with something, I’d look up a fast way to do it, learn it and continue on.

One thing I’ve learned is, there is always a faster way to do something in Vim.


### More reading
* [Thoughtbot's material on Vim is really great.](https://learn.thoughtbot.com/vim)
* Once you learn Vim, checkout Tmux - <a href="http://robots.thoughtbot.com/a-tmux-crash-course">A tmux Crash Course</a>
