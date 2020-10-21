+++
title = "Restarting with ox-hugo"
author = ["Kevin Pavao"]
date = 2020-10-20T23:43:00-05:00
draft = false
+++

## Hello World v2.0 {#hello-world-v2-dot-0}

I have had some extra time on my hands in the past few months due to COVID, so one of the things I decided to do to keep my mind occupied was to restart this site. My goal is to post something every few months at least, so I wanted to come up with at least three posts before I really thought about how I was going to handle the technical side of things.

I've found that I like to work on multiple posts/ideas at once for blogging, so I wanted to a way to keep everything in a single place. I decided to use a single `.org` file in its own git repository. This way I can see everything at a glance, quickly organize things, and keep it separate from the actual blog site. I can also take advantage of org's use of hierarchies for tags and properties and such.

As of now, my `blog-posts.org` has three main headers with posts beneath:

1.  **Published** - published posts, each post under this has a status of `DONE`
2.  **In Progress** - posts I am currently working on, each post under this has a status of `DRAFT`
3.  **Ideas** - any ideas for future posts, each idea under this has a status of `TODO`

Currently, **Ideas** is a dumping ground of things that I may or not make posts of, and some quick thoughts of things the post might contain. Once I have a good idea of what I want the post to be, I'll move it to **Published**, start outlining it, and go from there. It may get disorganized in the future, but it is working well for me so far.

Of course, I failed at my goal of not thinking about the technical side things, as I started working on generating static sites using Clojure. I have been looking for a good reason to really learn Clojure and this sounded like a great project for it, so I started hacking on something and I was able to get a prototype working, but I figured it would take too long to actually get it where I wanted.

That's when I realized that the org file I had would work perfectly with [ox-hugo](https://ox-hugo.scripter.co/). It allows you to keep all your posts in a single file and it uses org's export capability to output it to hugo parseable markdown. After reading the documentation of both `hugo` and `ox-hugo`, I was able to get something working super quickly. I only had to make minor changes to my org file.


## Using ox-hugo {#using-ox-hugo}

I was up and running after these 3 changes:

-   Add this to the top of the org file:

<!--listend-->

```org
#+hugo_base_dir: ../personal-website/
#+property: header-args :eval never-export
```

The file containing all my posts and the hugo site are both seperate folders in my `~/projects/` directory, so I am pointing `#+hugo_base_dir` to the hugo site there.

-   Add this under post headers:

<!--listend-->

```org
** Restarting with ox-hugo
:PROPERTIES:
:EXPORT_FILE_NAME: restarting-with-ox-hugo
:END:
```

-   Add the `:noexport:` tag to the **In Progress** and **Ideas** headers:

<!--listend-->

```org
* In Progress :noexport:
```

This way only posts beneath the **Published** header will work.

To have the date on the post, `ox-hugo` takes the date from the `DONE` status. You need have `org-log-done` set to `'time` in Emacs for this to work.

Now, I can press `C-c C-e H A` and the posts export all sub-headers under **Published** with the `:EXPORT_FILE_NAME:` property into `hugo` posts. Minimal changes to the file, and now I have a working site. Awesome!


## Org Capture {#org-capture}

Another advantage of having all the blog posts in a single file is that you can take advantage of capture templates. Luckily, `ox-hugo` provides some nice examples of them [here](https://ox-hugo.scripter.co/doc/org-capture-setup/).

I am just getting into using `org-capture` so I am using the one provided there modified to fit my file structure.


## Final Thoughts and the Future {#final-thoughts-and-the-future}

Now that I've got a setup I like, I will continue working on some posts and keep hacking away on the Clojure version of the site. I really like how all the posts are in a single file right now, so I'd like to keep using `ox-hugo` (or a modified version of it). I am still in the early stages of things though, so we will see where it goes!

So... Stay tuned! I have some posts on getting started with StumpWM and configuring Emacs for writing in the works.
