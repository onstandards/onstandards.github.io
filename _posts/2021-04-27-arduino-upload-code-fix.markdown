---
layout: post
title: "MXChip: Upload Code MacOSX issue"
date: 2021-04-27 1:27:00 -0000
categories: MXChip
---


{% highlight bash %}brew tap SomaticLabs/gnu-arm-toolchain{% endhighlight %}
{% highlight bash %}brew install gcc-arm-none-eabi openocd{% endhighlight %}
{% highlight bash %}cd ~/Library/Arduino15/packages/AZ3166/tools/openocd{% endhighlight %}
{% highlight bash %}mv 0.10.0 0.10.0.bak{% endhighlight %}
{% highlight bash %}mkdir 0.10.0{% endhighlight %}
{% highlight bash %}cd 0.10.0{% endhighlight %}
{% highlight bash %}ln -s /usr/local/Cellar/open-ocd/0.10.0 macosx{% endhighlight %}
{% highlight bash %}cd macosx{% endhighlight %}
{% highlight bash %}ln -s ~/Library/Arduino15/packages/AZ3166/tools/openocd/0.10.0.bak/scripts .{% endhighlight %}
{% highlight bash %}cd ~/Library/Arduino15/packages/AZ3166/tools/arm-none-eabi-gcc{% endhighlight %}
{% highlight bash %}mv 5_4-2016q3 5_4-2016q3.bak{% endhighlight %}
{% highlight bash %}ln -s /usr/local/Cellar/gcc-arm-none-eabi/20160926/ 5_4-2016q3{% endhighlight %}