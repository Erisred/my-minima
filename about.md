---
layout: page
title: About
permalink: /about/
---
This is your page. Tell us about who you are, what you do, or what you know. Folks will probably use this page to learn more about you, your brand, or your products.

---

This is the base Jekyll theme. You can find out more info about customizing your Jekyll theme, as well as basic Jekyll usage documentation at [jekyllrb.com](https://jekyllrb.com/)

You can find the source code for Minima at GitHub:
[jekyll][jekyll-organization] /
[minima](https://github.com/jekyll/minima)

You can find the source code for Jekyll at GitHub:
[jekyll][jekyll-organization] /
[jekyll](https://github.com/jekyll/jekyll)


[jekyll-organization]: https://github.com/jekyll



<ul class="contact-section">
<li style="font-size: 20px;">Contact</li>
<li>If you'd like to contact me:</li>

<li>{% if site.author.email -%}
    <a class="u-email" href="mailto:{{ site.author.email }}">{{ site.author.name }}</a>
{%- endif %}</li>
</ul>