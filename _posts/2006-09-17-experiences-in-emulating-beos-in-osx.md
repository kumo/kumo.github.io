---
title: "Experiences in emulating BeOS in OSX"
category: BeOS
layout: post
redirect_from: "/articles/2006/09/17/experiences-in-emulating-beos-in-osx/"
---
<p class="message">
Update: it is probably a much better to look at the official <a href="https://www.haiku-os.org/guides/virtualizing">Haiku wiki</a> for more up-to-date information.
</p>

A couple of nights ago I decided that it was time to see if I could get BeOS to work on my iBook (it worked but it was very slow). The only way I could get it to work was with emulation. I found the information on various places on the Internet but I forgot to note down where I found it from so I am recreating it here (so that at least I don't forget how it was done).

For now I am trying to use just the simple **BeOS 5 Personal Edition**.

- Download the BeOS R5 Personal Edition for Linux from [BeBits](http://www.bebits.com/app/2680)
- Extract the file that you just downloaded and if you want to, place `image.be` and `floppy.img` somewhere
- Install qemu which is a PC emulator. I installed it from [http://stegefin.free.fr/qemu/qemu.dmg](http://stegefin.free.fr/qemu/qemu.dmg) as suggested on the [Haiku Wiki](https://www.haiku-os.org/guides/virtualizing)

At this point, you should open up a Terminal window and `cd` to the location where `image.be` and `floppy.img` are. Both files are needed to get BeOS PE to run and so the following should be typed:

{% highlight sh %}
qemu -fda floppy.img -hda image.be -boot a -m 128 -user-net

# -fda floppy.img
# The floppy disk is floppy.img

# -hda image.be
# The hard disk is image.be

# -boot a
# Boot from drive a

# -m 128
# Provide 128 meg of memory

# -user-net
# Creates a private network (?)
{% endhighlight %}

After a while BeOS will be running in greyscale. Either I would have to enable the safe video mode everytime I booted BeOS or I would have to use one of the pieces of software on BeBits to force the video mode. I couldn't get the network to work so I was stuck with the problem of how to transfer files to BeOS. I tried to make a dmg but it didn't work so in the end I made a CD image using the following command:

{% highlight sh %}
hdiutil makehybrid -o stuff_for_beos.iso -iso DIRECTORY
{% endhighlight %}

At that point the CD image can be passed to qemu by adding -cdrom ISONAME to the qemu command:

{% highlight sh %}
qemu -fda floppy.img -hda image.be -cdrom stuff_for_beos.iso -boot a -m 128 -user-net
{% endhighlight %}
