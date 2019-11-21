User-facing documentation is maintained as a set of articles ("posts") using [Jekyll](https://jekyllrb.com).  These posts are maintained as part of the repo itself, under the `gh-pages` branch.

A deployed version of the Jekyll site is hosted using [GitHub Pages](https://help.github.com/en/github/working-with-github-pages), and is accessible at both [armandofox.github.io/audience1st] (canonical GitHub Pages URL) and the domain alias [docs.audience1st.com].

In case you're unfamiliar with Jekyll: 

* Each file in the `_posts` directory contains one "help article".  Each file has a name such as `2019-10-25-some-help-topic.md`.  
* Each file is self-contained, giving the title and text of the article (using  [Markdown](https://www.markdowntutorial.com/)), the article's category, and display order within the category.  Here is what a typical file looks like.  The top (header) portion must be delimited by three dashes and must include at least the below fields as a minimum ("layout" can always be set to `page` for now; you can define custom layout templates later that look nicer):

```
---
layout: page
title: "Ticket Sales"
category: Reports
date: 2017-11-04 14:10:46
order: 20
---

# Ticket Sales

To see advance sales for one or more productions, ...

# Subscription and Other Orders Needing Fulfillment

Subscription Counts, displayed at the bottom of ...

etc.
```

* The file `_config.yml` contains various setup options, including the names and labels for the categories.

The files are all maintained in the `gh-pages` branch of this repo, so pushing to that branch automatically updates the GitHub Pages site (within a few minutes anyway).

To contribute documentation, you can either modify the files directly using a text editor and then commit and push to the `gh-pages` branch, or use a graphical front end like [Forestry](https://forestry.io) to edit and push.



