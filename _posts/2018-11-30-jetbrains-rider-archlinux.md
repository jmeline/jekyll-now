---
title: Installing Jetbrains' Rider in Archlinux
---

Today I fought long and hard to install jetbrains' Rider C# IDE onto my Archlinux machine. I think the reason why it was difficult was that I am using Bspwm. Here is what I did to get it working.

Install rider from Aur

{% highlight shell %}
$ git clone https://aur.archlinux.org/rider.git
$ cd rider
$ makepkg -sri
{% endhighlight %}

Install dotnet core
{% highlight bash %}
$ sudo pacman -S dotnet-sdk
{% endhighlight %}

Since Jetbrains implements its own version of Java runtime, you won't need to.

I had to add the following code to my .xinitrc since I am using bswpm. Java for some reason doesn't draw correctly unless I put the following export in my .xinitrc.

```
export _JAVA_AWT_WM_NONREPARENTING=1
```

## Adding .net core to the settings in order to build .net core projects.

I found that another thing that took me a while to get right was adding the script path for dotnet core to use.

{% highlight shell %}
$ /opt/dotnet/dotnet
{% endhighlight %}

This was the path that I added to the Rider settings in order to create a dotnet core project.

It worked for me after I performed these steps.
