---
layout: post
title: "Create demo certificates to test IoT Edge device features"
date: 2021-04-26 12:31:00 -0000
categories: IoTEdge, Certifiactes
---


IoT Edge devices require certificates for secure communication between the runtime, the modules, and any downstream devices. If you don't have a certificate authority to create the required certificates, you can use demo certificates to try out IoT Edge features in your test environment. This article describes the functionality of the certificate generation scripts that IoT Edge provides for testing.

These certificates expire in 30 days, and should not be used in any production scenario.

You can create certificates on any machine, and then copy them over to your IoT Edge device. It's easier to use your primary machine to create the certificates rather than generating them on your IoT Edge device itself. By using your primary machine, you can set up the scripts once and then use them to create certificates for multiple devices.

Follow these steps to create demo certificates for testing your IoT Edge scenario:

Set up scripts for certificate generation on your device.
Create the root CA certificate that you use to sign all the other certificates for your scenario.
Generate the certificates you need for the scenario you want to test:
Create IoT Edge device identity certificates for provisioning devices with X.509 certificate authentication, either manually or with the IoT Hub Device Provisioning Service.
Create IoT Edge device CA certificates for IoT Edge devices in gateway scenarios.
Create downstream device certificates for authenticating downstream devices in a gateway scenario.


It is possible to configure your Raspberry Pi to allow access from another computer without needing to provide a password each time you connect. To do this, you need to use an SSH key instead of a password. To generate an SSH key:

Check for existing SSH keys
===========================
First, check whether there are already keys on the computer you are using to connect to the Raspberry Pi:

{% highlight bash %}
ls ~/.ssh
{% endhighlight %}

If you see files named `id_rsa.pub` or `id_dsa.pub` then you have keys set up already, so you can skip the 'Generate new SSH keys' step below.

Generate new SSH keys
======================

To generate new SSH keys enter the following command:

{% highlight bash %}
ssh keygen
{% endhighlight %}

Upon entering this command, you will be asked where to save the key. We suggest saving it in the default location (`~/.ssh/id_rsa`) by pressing `Enter`.

You will also be asked to enter a passphrase, which is optional. The passphrase is used to encrypt the private SSH key, so that if someone else copied the key, they could not impersonate you to gain access. If you choose to use a passphrase, type it here and press `Enter`, then type it again when prompted. Leave the field empty for no passphrase.

Now look inside your `.ssh` directory:

{% highlight bash %}
ls ~/.ssh
{% endhighlight %}

and you should see the files `id_rsa` and `id_rsa.pub`:

`authorized_keys  id_rsa  id_rsa.pub  known_hosts`
The `id_rsa` file is your private key. Keep this on your computer.

The `id_rsa.pub` file is your public key. This is what you share with machines that you connect to: in this case your Raspberry Pi. When the machine you try to connect to matches up your public and private key, it will allow you to connect.

Take a look at your public key to see what it looks like:

{% highlight bash %}
cat ~/.ssh/id_rsa.pub
{% endhighlight %}

It should be in the form:

{% highlight bash %}
ssh-rsa <REALLY LONG STRING OF RANDOM CHARACTERS> user@host
{% endhighlight %}

Copy your public key to your Raspberry Pi
===========================


Using the computer which you will be connecting from, append the public key to your `authorized_keys` file on the Raspberry Pi by sending it over SSH:

{% highlight bash %}
cat ~/.ssh/id_rsa.pub | ssh <USERNAME>@<IP-ADDRESS> 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
{% endhighlight %}


If you see the message `ssh: connect to host <IP-ADDRESS> port 22: Connection refused and` you know the `IP-ADDRESS` is correct, then you may not have enabled SSH on your Raspberry Pi. Run `sudo raspi-config` in the Pi's terminal window, enable SSH, then try to copy the files again.

Now try `ssh <USER>@<IP-ADDRESS>` and you should connect without a password prompt.

If you see a message "Agent admitted failure to sign using the key" then add your RSA or DSA identities to the authentication agent `ssh-agent` then execute the following command:


{% highlight bash %}
ssh-add
{% endhighlight %}

If this does not work, you can get assistance on the [Raspberry Pi forums].

Note: you can also send files over SSH using the `scp` command (secure copy). See the [SCP guide] for more information.

Adjust permissions for your home and .ssh directories
=====================================================


If you can't establish a connection after following the steps above there might be a problem with your directory permissions. First, you want to check the logs for any errors:


{% highlight bash %}
tail -f /var/log/secure
# might return:
Nov 23 12:31:26 raspberrypi sshd[9146]: Authentication refused: bad ownership or modes for directory /home/pi
{% endhighlight %}

If the log says `Authentication refused: bad ownership or modes for directory /home/pi` there is a permission problem regarding your home directory. SSH needs your home and `~/.ssh` directory to not have group write access. You can adjust the permissions using `chmod`:

{% highlight bash %}
chmod g-w $HOME
chmod 700 $HOME/.ssh
chmod 600 $HOME/.ssh/authorized_keys
{% endhighlight %}

Now only the user itself has access to `.ssh` and `.ssh/authorized_keys` in which the public keys of your remote machines are stored.

Store the passphrase in the macOS keychain
==========================================


If you are using macOS, and after verifying that your new key allows you to connect, you have the option of storing the passphrase for your key in the macOS keychain. This allows you to connect to your Raspberry Pi without entering the passphrase.

Run the following command to store it in your keychain:


{% highlight bash %}
ssh-add -K ~/.ssh/id_rsa
{% endhighlight %}

[Raspberry Pi forums]:https://www.raspberrypi.org/forums/
[SCP guide]:https://www.raspberrypi.org/documentation/remote-access/ssh/scp.md