+++
title = ".emacs Refinancing (or creating the base of an Emacs config)"
author = ["Kevin Pavao"]
date = 2020-11-22T14:13:00-06:00
draft = false
+++

## Whether to Declare Bankruptcy or to Refinance {#whether-to-declare-bankruptcy-or-to-refinance}

In the Emacs community, there is something called [.emacs bankruptcy](https://www.emacswiki.org/emacs/DotEmacsBankruptcy), which is when your init file gets too large and cumbersome that you need start over from scratch. As someone who tends to love messing with config files a little too much, I knew that I would eventually run into this if I started really using Emacs. That is why, when I really started to use Emacs, started with a literate config file in `org-mode` and used `use-package` for configuration.

It works really well, but after several years, I started to realize it was beginning to fill with cruft, my startup time seemed to be increasing, and I was running into some random issues. I have also learned a bit more about Emacs since I started, so I figured it was time to take another look at things and see what I could improve.

I didn't want to declare "bankruptcy", as there are still many things I want to keep, but there were some parts I knew that I needed  to clean up. So I decided to "refinance" -- instead of starting over, I wanted to keep the core of what I had, and repay some of the "technical debt" I had built up. I started a new org file and copied over some of my "base" settings and packages.  I also decided to start only with basic Emacs stuff, no language setup. I went through the documentation of everything, removed things I thought were unnecessary, put everything I could into `use-package`, and then wrote what it does and why I was using it.


## `basemacs` {#basemacs}

After working on it for a while and seeing some posts on [r/emacs](https://www.reddit.com/r/emacs/) asking how to build your own config like doom or spacemacs, I figured someone could get some use out of this besides me. Thus, [basemacs](https://github.com/kwpav/basemacs) was born.

I've tried to keep it as unopinionated as possible, it sets some sane defaults and uses a minimal set of (but what I would consider essential) packages like `magit`, `company`, `flycheck`, and `which-key`. It's still a work in progress: I should probably add some better documentation/description for things, I am considering adding modules, and there are probably some other things I could include and improve. But there it is, maybe someone will find it useful.
