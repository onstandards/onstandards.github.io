---
layout: post
title: "Install or uninstall Azure IoT Edge for Raspberry PI"
date: 2021-04-18 11:37:00 -0000
categories: IoT Edge
---

Prepare your device to access the Microsoft installation packages.

Install the repository configuration that matches your device operating system.

{% highlight bash %}
curl https://packages.microsoft.com/config/debian/stretch/multiarch/prod.list > ./microsoft-prod.list
{% endhighlight %}

Copy the generated list to the sources.list.d directory.

{% highlight bash %}
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
{% endhighlight %}

Install the Microsoft GPG public key.

{% highlight bash %}
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
{% endhighlight %}

Install a container engine
==========================

Update package lists on your device.
{% highlight bash %}
sudo apt-get update
{% endhighlight %}


Install the Moby engine.

{% highlight bash %}
sudo apt-get install moby-engine
{% endhighlight %}

Generate configs for Raspberry PI
{% highlight bash %}
sudo modprobe configs
{% endhighlight %}


If you get errors when installing the Moby container engine, verify your Linux kernel for Moby compatibility. Some embedded device manufacturers ship device images that contain custom Linux kernels without the features required for container engine compatibility. Run the following command, which uses the check-config script provided by Moby, to check your kernel configuration:

{% highlight bash %}
curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
chmod +x check-config.sh
./check-config.sh
{% endhighlight %}


Install IoT Edge
================

Update package lists on your device.

{% highlight bash %}
sudo apt-get update
{% endhighlight %}

Check to see which versions of IoT Edge are available.

{% highlight bash %}
apt list -a aziot-edge
{% endhighlight %}

If you want to install the most recent version of IoT Edge, use the following command that also installs the latest version of the identity service package:

{% highlight bash %}
sudo apt-get install aziot-edge
{% endhighlight %}

Or, if you want to install a specific version of IoT Edge and the identity service, specify the versions from the apt list output. Specify the same versions for both services. For example, the following command installs the most recent version of the 1.2 release:

{% highlight bash %}
sudo apt-get install aziot-edge=1.2* aziot-identity-service=1.2*
{% endhighlight %}


[Original]

[Original]: https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge?view=iotedge-2020-11
