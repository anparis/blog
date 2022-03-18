---
title: "How I created this blog, with Hugo" 
date: 2022-02-18T17:09:29+01:00
draft: false
author: Antonin Paris
---

In this post, I will just give a quick overview and hints of how I created the website with Hugo, I will not go into all the details. My main inspiration and what I was aiming for this blog was [John L. Godlee blog](http://johngodlee.xyz/). A very well done blog in my opinion and I can only recommand you to take a look at his blog.

For this blog, I actually wanted to understand how hugo worked, so I started from scratch with hugo basic theme and used the book *"Brian P. Hogan - Build Websites with Hugo Fast Web Development with Markdown ( 2020, Pragmatic Bookshelf )*" which is like an 'how-to' guide and helped me to understand layouts in Hugo. I also used Hugo documentation.

The hugo files are available on my github in the 'blog' repository.

### The main problems
* Date format for my blog posts
* Find a way to only display the first 10 posts on my index page
* Formatting of my code blocks, code highlighting
* Add clickable icons in the footer

### The solutions

/**date formating problem**

It was actually well documented on https://gohugo.io/functions/format/ but I spend some time to find it.

I added `{{ .Date.Format "2006-01-02"  }}` in front of my blog post titles in my *list.html* file  :
```html 
{{ .Date.Format "2006-01-02"  }} - <a href="{{ .RelPermalink }}">{{ .Title }}</a>
```

/**posts display on index page problem**

To solve this problem I added these lines to my `index.html` file :
```html
<section class="posts">
  <ul>
    {{ range first 10 (where .Site.RegularPages "Type" "in" "posts").ByDate.Reverse }} 
    <li>
      {{ .Date.Format "2006-01-02"  }} - <a href="{{ .RelPermalink }}">{{ .Title }}</a>
    </li>
    {{ end }}
  </ul>
</section>
```
  Where :

  `range first 10` to display 10 posts only.

  `.ByDate.Reverse` to display the posts from the latest to the oldest.


/**code block problem**

To custom code blocks, I added 2 lines in my `config.toml` to make syntax highlighting work.
```plain
pygmentsUseClasses = true
pygmentsCodefences = true
```
I chosed a [chroma style](https://xyproto.github.io/splash/docs/) to color my code blocks. Personally, I chosed the style *friendly* and added it with this command :
```shell
hugo gen chromastyles --style=friendly > syntax.css
```
Then I added syntax.css in my `head.html` file 
```html
<link rel="stylesheet" href="{{ "/syntax.css" | relURL }}" />
```

And then I just couldn't find how to make it work until I changed the location of syntax.css in my `blog/anparis/themes/basic/static/css/` folder which was before located in `blog/anparis/themes/basic/` and then it worked !

PS : Don't forget to change the path `/syntax.css` into `css/syntax.css` in `head.html`.


/**clickable icons problem**

Once that I discovered the existence of [font awesome](https://fontawesome.com/), adding icons worked like a charm.

To use it, I subscribed to receive a script that I could then add to my head.html (No idea if you can manage to make it work without subscription).

Then you just have to copy paste the icon that you want in your html code, for example the github icon :
```html
<i class="fa-brands fa-github"></i>
```
And the icon will appear on your website and you can customize it with your css stylesheet.
