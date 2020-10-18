+++
title = "Pelican and Org Mode"
author = ["Kevin Pavao"]
date = 2018-09-23T22:24:00-05:00
draft = false
+++

One of the first things you need to do after deciding to start a blog, is setting one up. So I figured it would be appopropriate that the first post of my blog is about how I set this up.

My requirements are fairly simple:

-   be able to write content using Emacs' `org-mode`
-   create a static site
-   preferably written in a language I know

This led me down a path of looking at and trying a few different static site generators.

Each static site generator essentially works the same. There is a CLI which allows you to:

-   Scaffold out the site.
-   Create content in a folder in some sort of text format, e.g. markdown, rest, org, etc.
-   Generate HTML from the content
-   Run a local server to preview the website

After trying out a few different ones, I decided to use [Pelican](http://docs.getpelican.com/en/stable/). It seems to be the most popular and actively maintained one written in Python. It's even used by [kernel.org](https://kernel.org)!


## Initial setup {#initial-setup}

First, install `pelican` (I use `pipenv` personally).

```python
pipenv install pelican
pipenv shell
```

Create the folder where you want the website and initialize the site:

```python
pelican-quickstart
```

This scaffolds out the files and directories needed.

Create a new post in the `content` folder.

Generate the output:

```python
pelican content
```

This creates generates the output into the `output` folder.

Preview the content:

```sh
cd output
python -m pelican.server
```

If you select "yes" during `pelican-quickstart`, a `Makefile` is created. This allows you to use `make` to automate some of the pelican tasks such as generating the output, publishing, running the server, etc.

See how to use it from the [official docs](http://docs.getpelican.com/en/stable/publish.html?highlight=makefile#make).


## Using org-mode {#using-org-mode}

In order to use `org-mode`, you need to install a plugin.

To install a plugin, clone the [pelican-plugins](https://github.com/getpelican/pelican-plugins) repo:

```sh
git clone --recursive https://github.com/getpelican/pelican-plugins
```

Add the path and the plugin to to your `pelicanconf.py`:

```python
PLUGIN_PATHS = ['path/to/pelican-plugins']
PLUGINS = ['org_reader']
```

There are three different plugins to choose from:

1.  `org_reader`
2.  `org_python_reader`
3.  `org_pandoc_reader`

Each plugin uses a different mechanism to convert the `.org` files to HTML. `org_reader` uses Emacs, `org_python_reader` uses the [orgco](https://github.com/paetzke/orgco) Python library, and `org_pandoc_reader` uses (you guessed it...) [Pandoc](https://pandoc.org).

It seems that `org_python_reader` is an 'update' for `org_reader`. `org_pandoc_reader` on the other hand looks to be based off of another plugin, `pandoc_reader`.

I tried out `org_reader` and `org_python_reader` but found some bugs. They were pretty easy to fix, so I decided to make my very first PR to a project that wasn't mine or a friends!

<https://github.com/getpelican/pelican-plugins/pull/1064>


### org\_reader setup {#org-reader-setup}

Add the following to `pelicanconf.py`:

```python
PLUGINS = ['org_reader']
ORG_READER_EMACS_LOCATION = '/usr/bin/emacs'
```

In order to define the metadata for Pelican, put the following in the `org` file's header:

```org
#+TITLE: The Title Of This BlogPost
#+DATE: 2001-01-01
#+CATEGORY: blog-category
#+AUTHOR: My Name
#+PROPERTY: LANGUAGE en
#+PROPERTY: SUMMARY hello, this is the description
#+PROPERTY: SLUG test_slug
#+PROPERTY: MODIFIED [2015-12-29 Di]
#+PROPERTY: TAGS my, first, tags
#+PROPERTY: SAVE_AS alternative_filename.html
```


### org\_python\_reader setup {#org-python-reader-setup}

Add the following to `pelicanconf.py`

```python
PLUGINS = ['org_python_reader']
ORGMODE = {
        'code_highlight': True,
}
```

It will not work unless you add `ORGMODE`. The only option currently is `code_highlight` which can be set to `True` or `False`. This tells the plugin whether to add syntax highlighting to `SRC` blocks.

This uses the same `org` headers as `org_reader`.

One thing to note is that this plugin will always print out line numbers in `SRC` blocks due to its dependency on [orgco](https://github.com/paetzke/orgco).


### org\_pandoc\_reader setup {#org-pandoc-reader-setup}

In order to use [org\_pandoc\_reader](https://github.com/jo-tham/org%5Fpandoc%5Freader/tree/bf06b72c1bfe1831f3e4c872f6c833af0bec19bf), you need to clone it as the `pelican-plugins` repo only links to it, it doesn't include it directly:

```sh
git clone https://github.com/jo-tham/org_pandoc_reader.git
```

Add the following to `pelicanconf.py`

```python
PLUGINS = ['org_pandoc_reader']
ORG_PANDOC_ARGS = ['--standalone',]
```

Without `--standalone`, the SRC blocks don't have syntax highlighting. Source code blocks are also underlined for me in some themes for some reason. I might take a look at why that is happening later.

`org_pandoc_reader` also does not use `PROPERTY` to generate metadata, you just use the name of the setting directly, e.g.:

```org
#+TITLE: The Title Of This BlogPost
#+DATE: 2001-01-01
#+CATEGORY: blog-category
#+AUTHOR: My Name
#+LANGUAGE: en
#+SUMMARY: hello, this is the description
#+SLUG: test_slug
#+MODIFIED: [2015-12-29 Di]
#+TAGS: my, first, tags
```


## Conclusion and other thoughts {#conclusion-and-other-thoughts}

Using Pelican and org mode is pretty nice once its set up. Although I did run into some troubles initially, and I was able to fairly easily fix the issues I was having. I even submitted my first issues and PR's to an open source project that wasn't my own or a friends!

-   [Fix the processing of org files with SRC blocks.](https://github.com/getpelican/pelican-plugins/pull/1066)
-   [Fix loading of org\_reader plugins.](https://github.com/getpelican/pelican-plugins/pull/1064)

In my testing of different static site generators, I tried [Hugo](https://gohugo.io/), written in go, along with the [ox-hugo](https://ox-hugo.scripter.co/) package for Emacs. I really liked it as `ox-hugo` gives you the ability to have several posts/pages/etc in a single org file. I would ultimately like to set something like that up for Pelican, but thats a task for a later day.
