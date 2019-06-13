---
title: Watching Blu-ray on Archlinux
---

I have been looking for a way to watch my Blu-ray movies on my Archlinux machine. I wanted to share what my path to success was. I utilized vlc and a few packages needed for decrypting Blu-rays.

First make sure you have vlc version>3.0 installed

{% highlight shell %}
$ sudo pacman -S vlc
{% endhighlight %}

I also had to grab a few packages from the AUR as vlc needs to have the proper packages for Blu-ray. I use Yay for my AUR needs
{% highlight shell %}
$ yay -S libbluray-git aacskeys
{% endhighlight %}

After installing those packages, I figured that vlc would just work. I ended up getting the following error when I tried to play a Blu-ray:

```
Blu-ray error:Missing AACS configuration file!
```

Following a lot of googling, I discovered the next set of instructions from this site: https://ubuntuforums.org/archive/index.php/t-2187010.html, it is geared for ubuntu but the concepts were relatable for those of us on Archlinux.

{% highlight shell %}
$ mkdir ~/.config/aacs/
$ cd ~/.config/aacs/
$ wget https://vlc-bluray.whoknowsmy.name/files/KEYDB.cfg
{% endhighlight %}

I ran into an issue where the last command would complain that the file had moved or was no longer available. I simply went to the https://vlc-bluray.whoknowsmy.name/ and picked up the needed KEYDB.cfg.

Once that was accomplished, I ran vlc and selected "Open Disk". This was all I needed and my movie worked.

Hopefully this helps someone as this took me some time to figure it out. I am finding more and more reasons why I don't need to keep Windows around.
