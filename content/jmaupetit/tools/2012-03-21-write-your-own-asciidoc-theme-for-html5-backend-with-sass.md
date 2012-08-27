Date: 2012-03-21
Title: Write your own AsciiDoc theme for HTML5 backend with SASS
category: Tools
Slug: write-your-own-asciidoc-theme-for-html5-backend-with-sass
tags: sass, css, html

<div class="intro" markdown="1">
A few months ago, I was searching for a good markup language to write documents for my [company](http://www.comsource.fr). Desired features I was looking for were:

* text based pivot markup language to ease collaborative work with a [version control system](http://en.wikipedia.org/wiki/Source_code_management "Source Code Management"), such as [git](http://git-scm.com/) or [mercurial](http://mercurial.selenic.com/)
* easy convertion to other file formats like HTML, PDF, etc.
* rich enough to define my own stylesheet/classes for the HTML backend. This is a crucial point.

To my knowledge, [AsciiDoc](http://www.methods.co.nz/asciidoc/) seems to answer to all of these requirements.
</div>

## AsciiDoc

As defined in the [AsciiDoc home page](http://www.methods.co.nz/asciidoc/):

> AsciiDoc is a text document format for writing notes, documentation, articles, books, ebooks, slideshows, web pages, man pages and blogs. AsciiDoc files can be translated to many formats including HTML, PDF, EPUB, man page.
>
> AsciiDoc is highly configurable: both the AsciiDoc source file syntax and the backend output markups (which can be almost any type of SGML/XML markup) can be customized and extended by the user.

In a few words, AsciiDoc is yet another markup language with its own [syntax](http://powerman.name/doc/asciidoc) and command line tool `asciidoc` (written in python!) used to compile your document to a specified backend. For example, 

    :::bash
    $ asciidoc -b html5 -a icons -a toc2 -a theme=flask {yourdocument}.adoc

This will generate a file `{yourdocument}.html` with the flask theme embedded.

## CSS to SASS

When dealing with CSS, I have the (good?) habit to avoid doing pure CSS! There are two major languages used to ease the heavy task of writing a stylesheet: [SASS](http://sass-lang.com/ "SASS Lang") and [LESS](http://lesscss.org/ "LessCSS"). I will not debate here about what is the best framework to use for your own case, you will find plenty of articles about this around the web (see for example the [smashing magazine review](http://coding.smashingmagazine.com/2011/09/09/an-introduction-to-less-and-comparison-to-sass/)). I have started using SASS even it seems less (!) popular than LESS since [twitter boostrap](http://twitter.github.com/bootstrap/) use it (personal thought).

### SASS: Syntaxically Awesome StyleSheets

[Compass](http://compass-style.org "Compass SASS framework") is a CSS authoring framework based on the [SASS language](http://sass-lang.com/ "Syntaxically Awesome StyleSheets"). In a few words, Sass is an extension of CSS3 adding some pretty cool and useful features such as nested rules, variables, mixins, etc. The language has two different syntaxes, SCSS (close to CSS) and SASS (indented syntax inspired by [Haml](http://haml-lang.com/ "Haml markup")). If you use to practice CSS, I think you would prefer SCSS (as I do!). Your SASS source will be compiled to a clean CSS thanks to the `compass` command line tool.

### The Compass framework

The Compass framework adds a lot of mixins to deal with CSS3, a grid system ([blueprint](http://blueprintcss.org/ "blueprint CSS")), your layout and more. For an up-to-date exhaustive list of implemented mixins, please, check out the [compass documentation](http://compass-style.org/reference/compass/).

### Let's start working

First, you need to install Compass (see [compass install instructions](http://compass-style.org/install/)), and then create a new compass project with:

    :::bash
    $ compass create AsciiDoc_SASS --using blueprint

The `--using blueprint` argument is optional; add it if you want to use the blueprint grid framework. Once created, your project will look like:

    AsciiDoc_SASS/
    ├── config.rb
    ├── images
    │   └── grid.png
    ├── sass
    │   ├── ie.scss
    │   ├── partials
    │   │   └── _base.scss
    │   ├── print.scss
    │   └── screen.scss
    └── stylesheets
        ├── ie.css
        ├── print.css
        └── screen.css

Since by default, AsciiDoc will only consider one single css file for your theme, you can remove unused print.{scss,css} and ie.{scss,css} files (please, tell me you are not using IE < 9).

Now, it's time to find a name for your theme! My theme is hightly inspired by the `flask` theme so let it sounds more frenchy, and call it `fiole`. You will need to rename your `screen.scss` file to `{yourtheme}.scss`.

By default, the html5 backend template file loads two javascript files: `asciidoc.js` and `{yourtheme}.js`, so create a `js` directory and fetch the latest version of [asciidoc.js](http://asciidoc.googlecode.com/hg/javascripts/asciidoc.js). Your project tree is now:

    AsciiDoc_SASS/
    ├── config.rb
    ├── images
	│   ├── grid.png
	├── js
	│   ├── asciidoc.js
	│   └── fiole.js
	├── sass
	│   ├── fiole.scss
	│   └── partials
	│       ├── _base.scss
	└── stylesheets
		└── fiole.css

We will now convert asciidoc default CSS to SASS, thanks to [css2sass](http://css2sass.heroku.com "Convert your CSS files to Syntaxically Awesome StyleSheets code!") service (source code of the project is available on [GitHub](https://github.com/jpablobr/css2sass "css2sass project sources")). To do so, fetch the latest release of the asciidoc default stylesheet:

    :::bash
    $ cd /tmp
    $ wget http://asciidoc.googlecode.com/hg/stylesheets/asciidoc.css

Then copy all the asciidoc.css file content to the [sass2css]((http://css2sass.heroku.com) form and click **"Convert 2 SCSS"**. Copy The lower textarea SCSS code to a new partial file:

    :::bash
    $ cd AsciiDoc_SASS
	$ touch sass/partials/_asciidoc.scss
	$ emacs sass/partials/_asciidoc.scss

Add paste the SCSS code to the `_asciidoc.scss` file. You will need to add this style definition to the main document, by adding:

    :::css
    @import "partials/asciidoc"
	
at the end of the `sass/{yourtheme}.scss` file. Now you are ready to compile stylesheets with:

    :::bash
    $ compass compile
	
A more efficient way to work is to use the:

    :::bash
    $ compass watch
	
command. In this case, each time you save your scss file, compass automatically compiles your source file to update your css.

## Create your own theme

Now you are ready to work on you own theme. Go create a partial called `_{yourtheme}.scss` in `partials` directory to override `_asciidoc.scss` scss rules, and add it to your main theme file: `{yourtheme}.scss`. 

    :::css
    @import "partials/asciidoc";
    @import "partials/fiole";
    @import "partials/h5bp_print";
	
Procede the same way for printing rules. A good idea is to create a `_print.scss` partial with [html5 boilerplate](http://html5boilerplate.com/) good practices for `@media print` rules.

*Work Work Work ... coffee ... Work Work Work ... done!*

Once your style is ready for production, use the following command to generate a minified version of your CSS file:

    :::bash
    $ compass compile -e production --force

## Use your new AsciiDoc theme

Now it's time to test your style on your AsciiDoc file (*e.g.* `lorem_ipsum.adoc`) by typing the command:

    :::bash
    $ asciidoc -b html5 -a linkcss \
	-a stylesdir=AsciiDoc_SASS/stylesheets \
	-a scriptsdir=AsciiDoc_SASS/js \
	-a themedir=$(PWD)/AsciiDoc_SASS/stylesheets \
	-a theme=fiole \
	lorem_ipsum.adoc

And tadaaaah!

## Resources of the tutorial

You will find all ressources of this tutorial on github: [https://github.com/jmaupetit/AsciiDoc_SASS](https://github.com/jmaupetit/AsciiDoc_SASS)

