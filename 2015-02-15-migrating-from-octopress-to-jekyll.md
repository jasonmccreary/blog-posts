---
layout: post
title: "Migrating from Octopress to Jekyll"
heading: "Migrating from Octopress to Jekyll"
date: 2015-02-15 11:37
comments: true
categories:
  - Main Thread
description: "Back to the basics with just Jekyll."
subheading: "Back to the basics with just Jekyll."
excerpt: "Back to the basics with just Jekyll."
subheading: "Back to the basics with just Jekyll."
---
Over the years, this blog transitioned from [a custom CMS](/2008/09/hello_world/) to WordPress [to Octopress](/2013/01/migrating-wordpress-octopress/) and now to [Jekyll](http://jekyllrb.com). Each migration has been an invaluable learning experience.

In the process of redesigning this blog, I made the decision to migrate from Octopress to Jekyll.

Now some of you may be thinking - isn't Octopress Jekyll? Why transition to Jekyll?

First, let me say I have nothing against Octopress. Octopress was my gateway to Jekyll. For that I am thankful.

However, now that I am familiar with Jekyll, I don't *need* Octopress. In fact, in the [authors own words](http://octopress.org/2015/01/15/octopress-3.0-is-coming/):

> Octopress is basically some guy's Jekyll blog [...] released as a single product, but it's a collection of plugins and configurations which are hard to disentangle.

Furthermore, Octopress last release was 2011. While I don't update this blog often, Octopress seems to be dead.

Ultimately the abstraction of Jekyll through Octopress is cost without benefit. Migrating to Jekyll made it easier to find a [blog theme](http://jekyllthemes.org) and afforded me the opportunity to use [GitHub pages](https://pages.github.com).

## The Migration
Since Octopress *is* Jekyll, much of the configuration remained the same. There were variables where I had to cross-reference the [documentation](http://jekyllrb.com/docs/configuration/). In addition, some of the custom [front-matter](http://jekyllrb.com/docs/frontmatter/) variables for my posts didn't match my new theme. I wrote a quick script to convert/duplicate variables.

I also lost some of the Octopress specific *features*, notably:

- Integration with Disqus
- Integration with Google Analytics
- Archive pages
- Deployment Tools

To be fair, all but the last item have more to do with the theme than Octopress or Jekyll. Nonetheless, I had to rebuild these features.

Adding integration for Disqus and Google Analytics was straightforward. I added some configuration variables to `_config.yml` and updated the [liquid templates](https://github.com/Shopify/liquid) in the theme to include the respective code snippets.

The archive pages were not as straightforward. By default, Jekyll does not include a complete archive page with pagination. Furthermore, it does not generate category archive pages.

Within Jekyll you have access to all posts through the `site.posts` variable. As such, I could create [my archive page](/archives/) simply by looping over `site.posts`.

{% raw %}
    {% for post in site.posts %}
        {% include post_preview.html %}
    {% endfor %}
{% endraw %}

I was willing to lose pagination. So this simple loop was fine. If more you can review [this](http://reyhan.org/2013/03/jekyll-archive-without-plugins.html) and [that](http://stackoverflow.com/questions/18669143/how-to-group-posts-by-date-on-home-page-in-jekyll).

For the category pages I created a [Jekyll Plugin](http://jekyllrb.com/docs/plugins/). Technically a [generator](http://jekyllrb.com/docs/plugins/#generators). While the documentation actually contains *CategoryGenerator*, I ported the [Category Generator from Octopress](https://github.com/imathis/octopress/blob/master/plugins/category_generator.rb).

Eventually I may deploy to GitHub pages. For now, I deploy to a web server using the following rsync.

    rsync -vrz --checksum --delete _site/ server:~/webroot/

Eventually, I'd like to turn this into a [rake task](http://blog.grayghostvisuals.com/workflow/deploying-jekyll-with-rake/). For now, it's easy enough to run.

