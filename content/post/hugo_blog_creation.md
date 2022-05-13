---
title: "How I created this blog, with Hugo" 
date: 2022-02-18T17:09:29+01:00
draft: false
author: Antonin Paris
---

In this post, I will just give a quick overview and hints of how I created the website with Hugo, I will not go into all the details. My main inspiration and what I was aiming for this blog was [John L. Godlee blog](http://johngodlee.xyz/).

For this blog, I actually wanted to understand how hugo worked, so I started from scratch with hugo basic theme and used the book *"Brian P. Hogan - Build Websites with Hugo Fast Web Development with Markdown ( 2020, Pragmatic Bookshelf )*" which is like an 'how-to' guide and helped me to understand layouts in Hugo. I also used Hugo documentation.

The hugo files are available on my github in the 'blog' repository.

### The main problems
* Date format for my blog posts
* Find a way to only display the first 10 posts on my index page
* Formatting of my code blocks, code highlighting
* Add clickable icons in the footer

### The solutions

#### Date format

It was actually well documented on https://gohugo.io/functions/format/ but I spend some time to find it.

I added {{ .Date.Format "2006-01-02"  }} in front of my blog post titles in my list.html file :

```html 
{{ .Date.Format "2006-01-02"  }} - <a href="{{ .RelPermalink }}">{{ .Title }}</a>
```

#### Posts display

To solve this problem I added these lines to my index.html file :
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


#### Code blocks

To custom code blocks, I added 2 lines in my config.toml to make syntax highlighting work.
```plain
pygmentsUseClasses = true
pygmentsCodefences = true
```
I chosed a [chroma style](https://xyproto.github.io/splash/docs/) to add colors to my code blocks. Personally, I opted for the style *friendly*. You can use this command to add it :
```shell
hugo gen chromastyles --style=friendly > syntax.css
```
Then I added syntax.css in my head.html file 
```html
<link rel="stylesheet" href="{{ "/syntax.css" | relURL }}" />
```

The `/syntax.css` file needs to be located in `blog/anparis/themes/basic/static/css/` to make syntax highlighting work.


#### Clickable icons

**EDIT** : I was using [font awesome](https://fontawesome.com/) to manage icons in my footer but I realized that it was making my website heavy and I don't want to rely on external libraries.

The replacement I found to font awesome is [iconify](https://icon-sets.iconify.design/). I can select my icon and take the svg code associated with it. 

Then I add the svg icons in my footer :
```html
<a class="icon" target="_blank" href="YOUR LINK">
    <svg xmlns="http://www.w3.org/2000/svg"
      width="1.03em" 
      height="1em" 
      viewBox="0 0 1536 1504">
      <path d="THE SVG PATH</svg>
</a>
```
And the icons will then appear on your website. You can customize them with css.
