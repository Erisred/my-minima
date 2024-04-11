---
layout: post
title: "Adding tag capability to the base Minima Jekyll template"
tags: test
---
The base Minima theme does not come with any tagging support, but it's pretty easy to add. Here is a bare-bones way.

## Build the list of tags
In the background, we need to have the site develop the list of tags, and make them available for use throughout. To do this, add a file to the `includes` folder named `collecttags.html` with the following content:

{% raw %}
```html
{% assign rawtags = "" %}
{% for post in site.posts %}
  {% assign ttags = post.tags | join:'|' | append:'|' %}
  {% assign rawtags = rawtags | append:ttags %}
{% endfor %}
{% assign rawtags = rawtags | split:'|' | sort %}

{% assign site.tags = "" %}
{% for tag in rawtags %}
  {% if tag != "" %}
    {% if tags == "" %}
      {% assign tags = tag | split:'|' %}
    {% endif %}
    {% unless tags contains tag %}
      {% assign tags = tags | join:'|' | append:'|' | append:tag | split:'|' %}
    {% endunless %}
  {% endif %}
{% endfor %}
```
{% endraw %}

This creates a list called `site.tags`.

Now, we need to execute the building of the list. Since it needs to happen *before* we can use the list, we need to put it in the `head` files. If your template has a file `custom-head.html`, put it there. Otherwise, the `head.html` file will do.

{% raw %}
```html
{% if site.tags != "" %}
  {% include collecttags.html %}
{% endif %}
```
{% endraw %}

## Displaying tags on the posts
In the `_includes` folder, make a file named `tagline.html` with the following content:

{% raw %}
```html
<span>Tags:
	{% for tag in page.tags %}
		{% capture tag_name %}{{ tag }}{% endcapture %}
	<a href="{{ site.baseurl }}/{{ tag_name }}"class="postTags"><nobr>
		{{ tag_name }}</nobr></a>&nbsp;
	{% endfor %}
</span>
```
{% endraw %}

Note that this also makes each displayed tag into a link, at /tag/tag_name

Also note the use of `site.baseurl` here. Most tutorials for tags/categories assume the site is located in the user’s base repository. Mine is not, so I cannot use the simple `site.url`. The base url needs to be set up in the `config.yml` file. See Jekyll documentation for details.

Put the following in the post template, in the location you wish the tags to be displayed: {% raw %}`{% include tagline.html %}`{% endraw %}.

Note: the links will be displayed identical to the rest of the links on the site. The class `postTags` can be altered to style these links as desired.

## Make the pages for tag posts
We need to create the pages these tag links point to. A new page needs to be created every time a new tag is used/created. There are scripts that can do this, but require a little more programming skill than this post will cover.

First, create the layout for the page so it can be repeated. In the `layouts` folder, create a file called `tagpage.html` with the following content:

{% raw %}
```html
---
layout: default
---

<header class="postHeader">
	<h1>Tag: {{ page.tag }}</h1>
  	{{ content }}
	<hr>
</header>

<ul>
	{% for post in site.tags[page.tag] %}
		<li><a href="{{ post.url | prepend:site.baseurl }}">{{ post.title }}</a>{{ post.date | date_to_string }})<br>
			{{ post.description }}
		</li>
	{% endfor %}
</ul>
```
{% endraw %}

This layout is now available to use on your tag pages. Each tag needs a place to display its contents. Like so:

{% raw %}
```html
---
layout: tagpage
title: "Tag page for TAGNAME"
tag: TAGNAME
---

These are posts that are tagged with the **{{ page.tag }}** tag.
```
{% endraw %}

Change the TAGNAME in both places to match the new tag. Each tag needs its own file, placed in the project's root named `tag.html`.

## Creating one page that lists all tags on the site.
In the root folder, create a page named `tags.html` with the following content:

{% raw %}
```html
---
layout: page
title: Site tags
---

<h1>Tags used on this site</h1>
<hr/>
{% for tag in site.tags %}
	<h3><a href="{{ site.baseurl }}/{{ tag[0] }}">{{ tag[0] }}</a></h3>
	<ul>
		{% for post in tag[1] %}
			<li><a href="{{ post.url | prepend:site.baseurl }}">{{ post.title }}
			</a></li>
		{% endfor %}
	</ul>
{% endfor %}
```
{% endraw %}

## Create a list of all tags on the site
Create a list of tags used on the site (without the list of associated posts). Each tag will link to its tag page.

Add a file to the _includes folder called `tagArchive.html` with the following:

{% raw %}
```html
<h2\>Archive</h2\>
{% capture temptags %}
	{% for tag in site.tags %}
		{{ tag\[1\].size | plus: 1000 }}#{{ tag\[0\] }}#{{ tag\[1\].size }}
	{% endfor %}
{% endcapture %}
{% assign sortedtemptags = temptags | split:' ' | sort | reverse %}
{% for temptag in sortedtemptags %}
	{% assign tagitems = temptag | split: '#' %}
	{% capture tagname %}{{ tagitems\[1\] }}{% endcapture %}
	<a href\="{{ siteurl | prepend:site.baseurl }}/tag/{{ tagname }}" class\="postTags"\><nobr\>{{ tagname }}</nobr\></a\>&nbsp;
{% endfor %}
```
{% endraw %}

To put this in a page or post, add {% raw %}`include tagArchive.html`{% endraw %} where you want it.