Date: 2012-02-20
Title: Hello Jekyll
Category: Web
Slug: 2012-02-20-hello-jekyll
Tags: jekyll, me, YetAnotherCoolStuff

<div class="intro" markdown="1">
After searching for months an easy to use blogging system for my everyday use, I finally found [Jekyll](http://jekyllbootstrap.com/ "Jekyll boostrap project page"), thanks to my [twitter](https://twitter.com/#!/julienmaupetit "Twitter Julien Maupetit") feeds.
</div>

<img src="http://mskstatic.com/386/515/medias/photos/programmes/moins_de150000/132996/dr-jekyll-and-mr-hyde.jpg" alt="Jekyll and Hide" />

## Features

With Jekyll, create a new blog post or a new page for your blog is as easy as typing a single command line like `rake do something`. You only need to focus on the content written in [markdown](http://daringfireball.net/projects/markdown/syntax "Markdown syntax"). Main advantages of this tool are:

* You only need your favourite **text editor** ([GNU Emacs](http://www.gnu.org/software/emacs/ "GNU Emacs Project page") of course!) and a terminal (with [git](http://git-scm.com/ "git SCM") installed) to publish new content on your blog
* **No database** is requiered
* Github **free hosting** (uses [GitHub Pages](http://pages.github.com/ "GitHub Pages"))
* GitHub Pages allows you to use **your own domaine name** (define a CNAME) pointing to your repository (your blog in fact)
* *Optionally you can locally install jekyll to preview your content*

## Overview of Jekyll

### Create a blog post

Once installed and configured (5 minutes with the [Jekyll quick start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html) guide), simply create a blog post with the command:

	:::bash
    $ rake post title="My New Post"

This will create a properly formated file `./_posts/2012-03-15-my-new-post.md`. This is the one you must edit with your post content.

### Create pages

You may also want to generate static pages for your blog. Let's say you want to create a static page with relevant informations about you, type:

	:::bash
    $ rake page name="about.md"

This will create your page template `about.md` in your project root directory. This page will be automatically listerd in the `/pages.html` view and available at `/about.html`. The Jekyll documentation mention the possibility to create nested pages via the command:

	:::bash
    $ rake page name="pages/about.md"

### Publish

Once configured, publishing new content to your blog is a classical git workflow:

	:::bash
    $ git add .
    $ git commit -am 'New blog post added'
    $ git push origin master

Now wait for a few minutes, GitHub will send you a notification once our page has been build successfully.

### Syntax highlighting

One of the major feature I liked is the opportunity to add syntax highlighting for code blocks: Jekyll uses [Liquid](http://www.liquidmarkup.org) to process templates and adds [its own tags](https://github.com/mojombo/jekyll/wiki/liquid-extensions "See Jekyll Liquid extensions") including `highlight`.

Using the `highlight` liquid tag, the following block code:

	:::text
	{% literal %}
    {% highlight python linenos %}
    print "Hello Jekyll!"
    {% endhighlight %}
	{% endliteral %}

will be rendered as:

	:::text
	{% highlight python linenos %}
	print "Hello Jekyll!"
	{% endhighlight %}


>Note to myself: to display the non-rendered block you should use the `literal` tag for liquid 2.2.2 and the `raw` tag for liquid 2.3.0, and both of them are not backward compatible... Hence, when `gh-pages` will upgrade to liquid 2.3.0, this page will be broken and will need an update. Please, dear reader: do not hesitate to warn me! This has been reported in [issue #425](https://github.com/mojombo/jekyll/issues/425)


### Configure your domain name

As explained in [GitHub Pages documentation](http://pages.github.com/#custom_domains "Read GitHub Pages Documentation"), GitHub offers you the possibility to use your own CNAME pointing to your repository/blog. Hence, at the root of your repository add a file named `CNAME` which contains your subdomain name, *e.g.*

    yageekblog.maupetit.net

And of course, configure your DNS settings properly, *e.g.*

    yageekblog.maupetit.net. CNAME IN jmaupetit.github.com.


## Still some work to do...

Now that I have found the technical solution I was looking for, I have to do some more work to migrate posts from my previous blog ( [http://www.maupetit.net/Blog/](http://www.maupetit.net/Blog/ "Julien Maupetit Deprecated Blog") ) and define my own template and not [the-minimum theme](http://themes.jekyllbootstrap.com/ "Theme for Jekyll-bootstrap").

## UPDATE - 2012/04/05 

### Debugging your pages/posts

In `page build failure` case (when pushing to github), install gem versions used in production by github, *e.g.* `liquid 2.2.2` and `jekyll 0.11.0`

	:::bash
    $ sudo gem install liquid -v 2.2.2
    $ sudo gem install jekyll -v 0.11.0

To see building errors, inactivate the `auto` mode, by editing your `_config.yml` as:

    auto: false

Always run the jekyll server with the `--safe` option
	
	:::bash
    $ jekyll --server --safe
