---
layout: post
title:  "First post"
date:   2021-01-30 20:20:20 +0100
categories: announcements
tags: math
---
{% include math.html %}

This is my first jekyll post, which can be found in the `_posts` directory. Such directory is available on the server side only. However, the default setup of this blog allows anonymous clients to navigate through the index of the blog static files. Just have a look at the [math index][math-index]. Math is a category and misc a subcategory of math. Is this a miscellaneous math post? You'll see at the end.

Edit: apparantly index browsing is only available when I run the blog locally.

<!--more-->

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Jeykll blog posts use YAML syntax, where a linebreak is achieved by keeping an empty line between two paragraphs. Adding further empty lines will not increase the space between paragraphs, it is therefore non-space sensitive after 1 line break. With regular spaces this is what                       happens (nothing as you can see). I have around 20 space characters in between "what" and "happens" on the original post ".markdown" file.

Jekyll also offers powerful support for code snippets.

Javascript:
{% highlight javascript %}
const hotline = new Audio("sounds/elevator.mp3");
document.addEventListener("keydown", toggleRotation);
function toggleRotation(e) {
  if (e.code == 'Space') {
    render = (render == freeze) ? animate : freeze;
    isRoatationToggled = !isRoatationToggled; //Roatate is def not a typo...
    hotline.play();
  }
}
{% endhighlight %}

Same code but in ruby (see the different formatting, I doubt it would compile...):
{% highlight ruby %}
const hotline = new Audio("sounds/elevator.mp3");
document.addEventListener("keydown", toggleRotation);
function toggleRotation(e) {
  if (e.code == 'Space') {
    render = (render == freeze) ? animate : freeze;
    isRoatationToggled = !isRoatationToggled; //Roatate is def not a typo...
    hotline.play();
  }
}
{% endhighlight %}

----

## Header 2 (H1 is reserved for post titles)

Jekyll is an active open source project that is updated frequently. If the github-pages gem on your computer is out of date with the github-pages gem on the GitHub Pages server, your site may look different when built locally than when published on GitHub. To avoid this, regularly update the github-pages gem on your computer.

1. Open Git Bash.
2. Update the github-pages gem.
    * If you installed Bundler, run bundle update github-pages. (this is my case, because I have Windows...)
    * If you don't have Bundler installed, run gem update github-pages.

And now the moment of truth... This is a math post because I decided to include a math snipet to showcase myself how to include "latex", or I'd rather say [Mathjax](https://math.stackexchange.com/editing-help), formatting:

 {% raw %}
 Block equation:

$$\sum_{i=0}^{n}\frac{1}{2^n} = 2$$

In-line equation: $\sum_{i=0}^{n}\frac{1}{2^n} = (\frac{1}{2^0})\cdot \frac{1}{1-\frac{1}{2}} = 2$
 {% endraw %}

 Using the raw liquid tag to ensure Markdown parsers do not interfere with MathJax syntax.

 ![Computer Science has always been about math][math-meme]
 *Insert image test*

That was the end of the post, below you won't see anything else, but in the markdown file I can see an additional line (with a proper line break before) with "\[math-index\]: /blog/announcement", which allows me to use `_[announcement-index]` as a variable with an assigned hyperlink. If I use one without a value assignment then I get this for instance: [Jekyll Talk][jekyll-talk].

Edit, just wanted to add a table:

To add a table, use three or more hyphens (---) to create each column’s header, and use pipes (\|) to separate each column. You can optionally add pipes on either end of the table.

| Syntax      | Description |
| ----------- | ----------- |
| Lorem       | Ipsum       |
| Paragraph   | Text        |
| Paragraph   | Text        |
| Paragraph   | Text        |

You can use pipes | To have a 2 column block of text

[math-index]: /math
[math-meme]:{{ site.url }}/images/cs_is_math.png