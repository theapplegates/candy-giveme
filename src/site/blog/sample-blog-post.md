---
layout: layouts/post-rsidebar.njk
title: Misc sample blog post
headTitlePost: "| Candy"
date: 2020-02-27
tags:
    - domain
    - frontmatter
---


**The setup**, you have your personal page at username.github.io with a custom domain, like custom.me. You have also created a project page for one of your repositories that by default would exist at custom.me/projectname.

This is assuming you have `projectname.io` already setup with the proper CNAME in the root of your site and your DNS working using CloudFlare.

Please note that the information below is just from trial and error and a whole bunch of Google searching. If you have any expertise and see any errors please let me know.

## A Less Than Ideal Solution

GitHub Pages doesn't offer any sort ability to setup redirects. So one option is to use JavaScript to do the redirect:

```html
<script>
  if (window.location.href.indexOf('http://custom.me') === 0) {
    window.location.href = 'http://projectname.io';
  }
</script>
```

Then use a meta tag to help search engines:

```html
<link rel="canonical" href="http://projectname.io"/>
```

If you are using Jekyll for example you could make that bit more flexible:

```html
<script>
  if (window.location.href.indexOf('http://custom.me') === 0) {
    window.location.href = 'http://projectname.io{{ page.url }}';
  }
</script>
```

```html
<link rel="canonical" href="http://projectname.io{{ page.url }}"/>
```

## A Much Better Solution

If you are using CloudFlare for your statically hosted blog great! If not, it is worth checking out. It provides two great features right off the bat. It puts all your assets in a CDN and allows you to setup 301 redirects. CloudFlare is also rolling out what they call CNAME flattening which should help with the 302 errors that occur when using an apex domain on GitHub Pages.

To setup the 301 redirect for your custom URL project page first you need to login to CloudFlare. Go to your Websites, click the gear icon then select Page rules.

You are going to Add new rule with Forwarding turned on. The URL pattern you enter would be something like, `custom.me/projectname/*`. Then the destination URL would be something like, `http://projectname.io/$1`.

Set Forwarding type to Permanent - 301 and click Add rule to apply your new redirect.

This nicer setup is directly taken from a great [post at Higher Order Heroku](http://www.higherorderheroku.com/articles/cloudflare-dns-heroku/). The post goes into how to setup an apex domain using a CNAME. This looks to be an older setup that could be done instead of doing the newer "CNAME flattening".
