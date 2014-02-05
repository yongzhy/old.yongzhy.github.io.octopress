---
layout: post
title: "Add \"Related Posts\" Section To Octopress"
date: 2014-02-05 12:39:53 +0800
comments: true
categories: Octopress
author: Zhu Yong
---

When reading someone's article, it's good to have a section to show the releated posts. Today I searched online and found to add `Related Posts` section to Octopress is quite easy. 

First, edit config file `_config.yml` to add line

    lsi: true

Second, create file `source/_includes/post/related_posts.html` with content below

{% raw %}
    {% if site.related_posts %}
      <h3>Related posts</h3>
      <ul class="posts">
      {% for post in site.related_posts limit:3 %}
        <li class="related">
          <a href="{{ root_url }}{{ post.url }}">{{ post.title }}</a>
        </li>
      {% endfor %}
      </ul>
    {% endif %}
{% endraw %}

Next, add this footer section to the post. modify `source/_layouts/post.html` to add line 

{% raw %}
    {% include post/related_posts.html %}
{% endraw %}

under 

{% raw %}
    {% include post/author.html %}
    {% include post/date.html %}{% if updated %}{{ updated }}{% else %}{{ time }}{% endif %}
    {% include post/categories.html %}
{% endraw %}

OK. Now you can run `rake generate` and `rake preview` to see the effects.

There is one problem with the solution above, it's slow to run `rake generate` when your have lots of articles. To speed up the process, you need to install `rb-gsl`. If you directly issue `gem install gsl`, your most probablly will see error message 

    Building native extensions.  This could take a while...
    ERROR:  Error installing gsl:
        ERROR: Failed to build gem native extension.
    
    ...

It's because `gsl` gem requires `gsl 1.14`. Follow the steps below to install `gsl 1.14` and `rb-gsl`:

    $ curl -O http://mirror.aarnet.edu.au/pub/gnu/gsl/gsl-1.14.tar.gz
    $ tar xfz gsl-1.14.tar.gz
    $ cd gsl-1.14
    $ ./configure
    $ make clean
    $ make                              # can take a while, be patient
    $ sudo make install
    $ sudo gem install gsl

Now run `rake generate` again and feel the acceleration for the classification process. If you are unlucky and see the message below:

    Notice: for 10x faster LSI support, please install http://rb-gsl.rubyforge.org/

This means your `rb-gsl` installation got problem. To resolve that, first uninstall `gsl` by 

    $ sudo gem uninstall gsl

Then edit file `Gemfile` in your Octopress folder to add line 

    gem 'gsl', '~>1.14.7'

Then use `bundle` to install `gsl`
 
    $ sudo bundle install

Again, run `rake generate` to feel the fly speed. :) 

Good Luck!!!