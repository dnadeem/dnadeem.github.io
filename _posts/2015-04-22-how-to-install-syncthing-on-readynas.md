---
layout: post
title: "How to install SyncThing on ReadyNas and Autostart"
tags:
- Readynas
- Syncthing
thumbnail_path: blog/syncthing-readynas/logo-text-256.png
---
Syncthing is Awesome. 

**Step 1:** Enable root access within ReadyNasOS 

{% include figure.html path="blog/syncthing-readynas/SSH.png" caption="This is how to enable root access" link_to_full_size_image=true %}


**Step 2:** Download the appropriate SyncThing image [here](https://github.com/syncthing/syncthing/releases)(I downloaded the ARM image because I have the ReadyNas 202).
<mark>NOTE:</mark> You must download the appropriate image or the installation will fail or cause errors later. If you do not know what type of CPU you have, simply click [here](http://www.netgear.com/business/products/storage/readynas/readynas-desktop.aspx#tab-models) and find your NAS model.

**Step 3** Download [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) and enter your IP address and login with a username of *root* and a specified password

**Step 4:**Type the following
{% highlight text %}cd ~
wget https://github.com/syncthing/syncthing/releases/download/v0.11.5/syncthing-linux-arm-v0.11.5.tar.gz{% endhighlight %}
Now, unpack the tar by pasting the following command:
{% highlight text %}tar xzvf syncthing*.tar.gz{% endhighlight %}
Now move into the dir
{% highlight text %}cd syncthing*{% endhighlight %}
Now lets call SyncThing like a service
{% highlight text %}cp syncthing /usr/local/bin{% endhighlight %}
Since we are done with the directory lets remove it
{% highlight text %}cd ~
rm -rf syncthing*{% endhighlight %}

**Step 5:** Lets start SyncThing
{% highlight text %}syncthing{% endhighlight %}
Let it run the setup for 2-3 minutes. Once you see a &#8220;Node ID,&#8221; Press ctrl + c and end the process.

**Step 6:** Edit the config file
{% highlight text %}nano ~/.config/syncthing/config.xml{% endhighlight %}
Look for the line below and change it to match the below image. 

{% include figure.html path="blog/syncthing-readynas/config.png" caption="Change it to 0.0.0.0" link_to_full_size_image=true %}


**Step 7:** Create a file named: &#8220;syncthing@.service&#8221; with following the contents and copy it to:
{% highlight text %}/etc/systemd/system{% endhighlight %}

{% highlight text %}
[Unit]
Description=Syncthing - Open Source Continuous File Synchronization for %I
Documentation=https://github.com/syncthing/syncthing/wiki
After=network.target
 
[Service]
User=%i
Environment=STNORESTART=yes
ExecStart=/usr/local/bin/syncthing -no-browser -logflags=0
Restart=on-failure
SuccessExitStatus=2 3 4
RestartForceExitStatus=3 4
 
[Install]
WantedBy=multi-user.target
{% endhighlight %}

**Step 8:** Let's implement autostart. Paste the following commands separately.
{% highlight text %}systemctl enable syncthing@root.service{% endhighlight %}
{% highlight text %}systemctl start syncthing@root.service{% endhighlight %}
Now check the status by entering:
{% highlight text %}systemctl status syncthing@root.service{% endhighlight %}
It should look like the image below:

{% include figure.html path="blog/syncthing-readynas/success.png" caption="Success!" link_to_full_size_image=true %}

**Step 9:** Go to http://ip:8080 and set up SyncThing.

**Step 10:** Reboot your NAS and it should start automatically.
<mark>NOTE:</mark> Do not type sudo when running these commands. You will get an error. SyncThing will automatically update to the latest version on first run.

