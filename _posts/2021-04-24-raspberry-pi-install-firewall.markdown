---
layout: post
title: "Raspberry PI: Install Firewall"
date: 2021-04-24 2:25:00 -0000
categories: Raspberry PI
---

Install a firewall
==================

There are many firewall solutions available for Linux. Most use the underlying iptables project to provide packet filtering. This project sits over the Linux netfiltering system. iptables is installed by default on Raspberry Pi OS, but is not set up. Setting it up can be a complicated task, and one project that provides a simpler interface than iptables is [ufw], which stands for 'Uncomplicated Fire Wall'. This is the default firewall tool in Ubuntu, and can be easily installed on your Raspberry Pi:

{% highlight bash %}
sudo apt install ufw
{% endhighlight %}

`ufw` is a fairly straightforward command line tool, although there are some GUIs available for it. This document will describe a few of the basic command line options. Note that `ufw` needs to be run with superuser privileges, so all commands are preceded with `sudo`. It is also possible to use the option `--dry-run` any `ufw` commands, which indicates the results of the command without actually making any changes.

To enable the firewall, which will also ensure it starts up on boot, use:

{% highlight bash %}
sudo ufw enable
{% endhighlight %}

To disable the firewall, and disable start up on boot, use:


{% highlight bash %}
sudo ufw disable
{% endhighlight %}

Allow a particular port to have access (we have used port 22 in our example):


{% highlight bash %}
sudo ufw allow 22
{% endhighlight %}

Denying access on a port is also very simple (again, we have used port 22 as an example):


{% highlight bash %}
sudo ufw deny 22
{% endhighlight %}

You can also specify which service you are allowing or denying on a port. In this example, we are denying tcp on port 22:


{% highlight bash %}
sudo ufw deny 22/tcp
{% endhighlight %}

You can specify the service even if you do not know which port it uses. This example allows the ssh service access through the firewall:

{% highlight bash %}
sudo ufw allow ssh
{% endhighlight %}

The status command lists all current settings for the firewall:


{% highlight bash %}
sudo ufw status
{% endhighlight %}

The rules can be quite complicated, allowing specific IP addresses to be blocked, specifying in which direction traffic is allowed, or limiting the number of attempts to connect, for example to help defeat a Denial of Service (DoS) attack. You can also specify the device rules are to be applied to (e.g. eth0, wlan0). Please refer to the ufw man page (man ufw) for full details, but here are some examples of more sophisticated commands.

Limit login attempts on ssh port using tcp: this denies connection if an IP address has attempted to connect six or more times in the last 30 seconds:


{% highlight bash %}
sudo ufw limit ssh/tcp
{% endhighlight %}

Deny access to port 30 from IP address 192.168.2.1


{% highlight bash %}
sudo ufw deny from 192.168.2.1 port 30
{% endhighlight %}


[ufw]: https://www.linux.com/training-tutorials/introduction-uncomplicated-firewall-ufw/
