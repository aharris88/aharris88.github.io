---
title: How to Redirect Pages in Jekyll
tags:
- jekyll
---

If you change the url of a page or post on WordPress, it automatically redirects from the old url to the new one. But since Jekyll is just static pages, it doesn't do this for you. I didn't really like WordPress doing this for me. Sometimes I didn't want or need it to do that. With Jekyll, you can be in full control, but you have to remember to do it.

There are a couple of ways to accomplish this, but both use the basic html tags to do the actually redirect. I prefer the second option, with layouts to make it even easier.

## Option 1- Create a new page for each redirect

If, for example, you changed a page from `videos/more-videos/index.html` to `media/videos/index.html`, then just put this in the old file.

{% highlight html %}
<html>
    <head>
	<meta http-equiv="refresh" content="0; url=http://www.lifestonechurch.net/media/videos/">
	<link rel="canonical" href="http://www.lifestonechurch.net/media/videos/" />
    </head>
</html>
{% endhighlight %}

## Option 2- Use Collections to Keep Redirects Organized

Create a new directory called `_redirects` and then in your `_config.yml` file, add this:

{% highlight bash %}
collections:
  redirects:
    output: true
    permalink: /:path/
{% endhighlight %}

Then all you have to do is add a new `.md` file for each redirect you need to do.

If, for example, you changed a page from `videos/more-videos/index.html` to `media/videos/index.html`, then just create a file called `_redirects/videos/more-videos.md`. And inside of it put:

{% highlight html %}
<html>
    <head>
	<meta http-equiv="refresh" content="0; url=http://www.lifestonechurch.net/media/videos/">
	<link rel="canonical" href="http://www.lifestonechurch.net/media/videos/" />
    </head>
</html>
{% endhighlight %}

## Use layouts to make it easier

But you can actually make this easier, no matter which method you use. you can create a layout called `_layouts/redirect.html` with the following in the file.

{% highlight html %}
---
---

<!doctype html>
<html>
    <head>
	<meta http-equiv="refresh" content="0; url={{page.newUrl}}">
	<link rel="canonical" href="{{page.newUrl}}" />
    </head>
</html>
{% endhighlight %}

And then in your `_config.yml`, you need to add a default layout for all the files in your `_redirects` collection. So add this at the bottom of the file:

{% highlight html %}
defaults:
  -
    scope:
      type: redirects
    values:
      layout: redirect
{% endhighlight %}

Then in your `_redirects` directory, the files can be simplified to this:

{% highlight html %}
---
newUrl: http://www.lifestonechurch.net/kids/birth-preschool/
---
{% endhighlight %}