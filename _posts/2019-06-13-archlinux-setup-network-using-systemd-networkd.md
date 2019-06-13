---
title: Setting up a new network in archlinux using systemd-networkd
---

{% highlight shell %}
$ ip link
{% endhighlight %}

{% highlight shell %}
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp9s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN mode DEFAULT group default qlen 1000
    link/ether 1c:1b:0d:09:f6:59 brd ff:ff:ff:ff:ff:ff
3: enp11s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 00:e0:4c:32:13:9a brd ff:ff:ff:ff:ff:ff
{% endhighlight %}

The interface I am interested in is enp11s0. enp9s0 has been my go to for a while now.

{% highlight shell %}
vi /etc/systemd/network/enp11s0.network
{% endhighlight %}

{% highlight shell %}

[Match]
name=en*
[Network]
DHCP=yes

{% endhighlight %}

save and exit

{% endhighlight %}
systemctl restart systemd-networkd
systemctl enable systemd-networkd
{% highlight shell %}

If you don't want to use systemd-networkd, you can use dhcpcd via systemctl directly

{% endhighlight %}
systemctl enable dhcpcd@enp11s0.service
systemctl start dhcpcd@enp11s0.service
{% highlight shell %}

All done!
