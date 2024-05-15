---
layout: post
author: Riven Skaye
title: "Atom feeds"
riven_says_thanks_to: https://rfong.github.io/rflog/2020/02/28/jekyll-tags/
---
{%- assign rawtags = "" -%}
{%- for post in site.posts -%}
  {%- assign ttags = post.tags | join:'|' | append:'|' -%}
  {%- assign rawtags = rawtags | append:ttags -%}
{%- endfor -%}
{%- assign rawtags = rawtags | split:'|' | sort -%}

{%- assign ignored = "" -%}
{%- if site.ignored_tags -%}
  {%- assign ignored = site.ignored_tags | join:'|' -%}
{%- endif -%}

{%- assign tags = "" -%}
{%- for tag in rawtags -%}
  {%- if tag != "" -%}
    {%- if tags == "" -%}
      {%- assign tags = tag | split:'|' -%}
    {%- endif -%}
    {%- unless tags contains tag or ignored contains tag -%}
      {%- assign tags = tags | join:'|' | append:'|' | append:tag | split:'|' -%}
    {%- endunless -%}
  {%- endif -%}
{%- endfor -%}

Using the nice and easy [`jekyll-feed` plugin](https://github.com/jekyll/jekyll-feed), this blog provides Atom feeds for people that would like to follow along for new posts. The feeds offered here are split on some of the tags. As series are ongoing, I'll make sure to set up a feed for them, then once the series reaches its end I'll yeet them sometime next update.

## Feeds currently available

- [Global feed for all posts](/feed.xml)
{%- if tags -%}
  {%- for feedtag in tags %}
- [{{ feedtag }} feed](/feed/{{ feedtag }}.xml)
  {% endfor -%}
{% endif %}
