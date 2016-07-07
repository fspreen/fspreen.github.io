---
layout:     post
title:      Starting Jekyll Without Gems
date:       Thu, 07 Jul 2016 16:45:30 -0400
---
As you can probably guess, I've begun a small blog here, hosted on GitHub Pages
and powered by Jekyll.  It's actually a little awkward because most of the
official documentation on Jekyll and GitHub Pages encourages users to set things
up by installing various Ruby gems, specifically the "github-pages" gem.  I
prefer to be a bit more hands-on than that, so I'll talk about what I did in
following my own path.  I'm assuming some familiarity with Git and the command
prompt here, as well as the basics of installing packages in Linux.

# Installing the Packages #
At the very beginning, I diverged from the instructions for both [Jekyll][] and
[GitHub Pages][] by obtaining my software from a different source.  The official
instructions encourage the user to make use of the "gem" command for managing
Ruby packages, called gems.

I don't know any Ruby (yet!), but I've had mixed experiences with the equivalent
features in Python.  I decided instead to install packages straight from the
Ubuntu repositories:

* git
* ruby
* jekyll

The dependency packages for Jekyll indicate that there is a lot going on under
the hood, despite the claims of simplicity.  In particular the "nodejs" package
caught my eye, though I don't know exactly what its role is with Jekyll.

Anyway, this has the advantage of packages and dependencies being tracked by the
package manager provided by Ubuntu, rather than any facility attached to Ruby.
This means I can update my software all from the same place, while Ruby can
focus on being a programming language.  The downside is that my version of
Jekyll may not fully match the one used by GitHub Pages.

# Initializing the Repository #
Initializing the repository and Jekyll build environment is fairly simple, but
the available documentation doesn't explain it.  I had to experiment a little
before finding the correct combination.

First, initialize a new Git repository:

`git init new-site`

As usual, this creates a new directory "new-site/" in the current working
directory.  Follow this with:

`jekyll new new-site`

This creates a new Jekyll project in the "new-site" directory.  If things worked
correctly, `new-site/.git/` and `new-site/_posts/` should both exist.

# Building and Serving #
With a working Jekyll project, running `jekyll build` will turn the templates
into a collection of static HTML pages.  Think of it as a translator or
compiler, similar to LaTeX or GCC.  Everything in the repository is processed by
Jekyll and the output files are placed in `_site` by default.

After building, the contents of `_site` could be copied to a folder to be served
up by Apache or nginx or some other web server.   It's even possible to tweak
the settings in `_config.yaml` to build in such a folder automatically.
However, the goal is to host on [GitHub Pages][] and so the `_site_` folder is
used for one thing:  invoking `jekyll serve` to run Jekyll's own server,
which will serve up the files in `_site` locally at `127.0.0.1:4000` by default.

# Committing to the Repository #
This is all well and good, but how much of the project needs to be committed?

The short answer:  almost everything.  Jekyll uses all those files to produce
the HTML files, so GitHub Pages needs all of the templates, CSS, etc. to do its
magic.  However, the generated files in `_site` should _not_ be committed.

Therefore, the last thing to do is to add a `.gitignore` file to exclude the
`_site` output directory from being accidentally committed.  I copied the
template `.gitignore` file in GitHub's collection repository,located
[here][jekyll_ignore].

And with that, Jekyll is up and running!

[Jekyll]: https://jekyllrb.com/ "Jekyll Website"
[GitHub Pages]: https://pages.github.com "GitHub Pages"
[jekyll_ignore]: https://github.com/github/gitignore/blob/master/Jekyll.gitignore
