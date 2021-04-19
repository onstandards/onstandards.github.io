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




***




# Provision the device with its cloud identity

Authenticate with symmetric keys

Create the configuration file for your device based on a template file that is provided as part of the IoT Edge installation.

{% highlight bash %}
sudo cp /etc/aziot/config.toml.edge.template /etc/aziot/config.toml
{% endhighlight %}

On the IoT Edge device, open the configuration file.

{% highlight bash %}
sudo nano /etc/aziot/config.toml
{% endhighlight %}

Find the Provisioning section of the file and uncomment the manual provisioning with connection string lines.

{% highlight bash %}
[provisioning]
source = "manual"
connection_string = "<ADD DEVICE CONNECTION STRING HERE>"
{% endhighlight %}

After entering the provisioning information in the configuration file, apply your changes:
{% highlight bash %}
sudo iotedge config apply
{% endhighlight %}

Verify successful configuration
===============================

Verify that the runtime was successfully installed and configured on your IoT Edge device.

Check to see that the IoT Edge system service is running.

{% highlight bash %}
sudo iotedge system status
{% endhighlight %}

A successful status response is Ok.

If you need to troubleshoot the service, retrieve the service logs.

{% highlight bash %}
sudo iotedge system logs
{% endhighlight %}

Use the check tool to verify configuration and connection status of the device.

{% highlight bash %}
sudo iotedge check
{% endhighlight %}

View all the modules running on your IoT Edge device. When the service starts for the first time, you should only see the edgeAgent module running. The edgeAgent module runs by default and helps to install and start any additional modules that you deploy to your device.

{% highlight bash %}
sudo iotedge list
{% endhighlight %}

Uninstall IoT Edge
==================

If you want to remove the IoT Edge installation from your device, use the following commands.

Remove the IoT Edge runtime.

{% highlight bash %}
sudo apt-get remove aziot-edge
{% endhighlight %}

Use the --purge flag if you want to delete all the files associated with IoT Edge, including your configuration files. Leave this flag out if you want to reinstall IoT Edge and use the same configuration information in the future.

When the IoT Edge runtime is removed, any containers that it created are stopped but still exist on your device. View all containers to see which ones remain.

{% highlight bash %}
sudo docker ps -a
{% endhighlight %}

Delete the containers from your device, including the two runtime containers.

{% highlight bash %}
sudo docker rm -f <container name>
{% endhighlight %}

Finally, remove the container runtime from your device.

{% highlight bash %}
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
{% endhighlight %}

[Original]

[Original]: https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge?view=iotedge-2020-11
