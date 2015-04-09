---
layout: post
title: "Learn to Bake: A Raspberry Pi Intro."
date: 2015-04-08 21:43:01 +1000
comments: true
categories: 
author: Ashley Gillman
---

A [Raspberry Pi](http://www.raspberrypi.org/ "Raspberry Pi Homepage") is what's called a SoC, a System on a Chip. This means it is an entire computer system on a single board, about the size of a credit card.
The Raspberry Pi can run various operating systems, most of which are some form of Linux distribution. The OS we will be using is called [Raspbian](http://www.raspbian.org/ "Raspbian Homepage"), and is a Debian derivative.

![Raspberry Pi Model A]({{ site.url }}/images/Raspberry_Pi_-_Model_A.jpg)

<!--more-->

Today's session will look at connecting to the Raspberry Pi using a protocol called [Secure Shell or SSH](http://en.wikipedia.org/wiki/Secure_Shell "SSH on Wikipedia"), which will allow us to connect to the Raspberry Pi using a PC or Mac. We could also plug a USB keyboard and mouse directly into the Raspberry Pi, along with a monitor and use it like a regular computer, but that's a little harder with a full class.

The first thing you will have to do is download [PuTTY](http://www.putty.org/ "PuTTY Homepage") (if you are on Windows, otherwise on Mac and Linux you already hace the ssh command). This will allow you to SSH, or connect, to the Raspberry Pi over command-line. Using the command-line can be a little scary at first, but it is a very powerful tool, and today will be mostly about getting comfortable with it. There are plenty of average-looking guides to shell, Unix, command-line and bash out there, [here](http://www.ee.surrey.ac.uk/Teaching/Unix/unixintro.html "Surrey EE Unix Intro") is one of them, [here](http://linuxcommand.org/learning_the_shell.php "LinuxCommand - Learning the Shell") is another.

Once you have PuTTY, run it, and enter the IP address of the Raspberry Pi (hopefully I have written it on the whiteboard by now). You will be prompted for a username (pi) and a password (raspberry). Hooray! You have connected to your first Raspberry Pi.

![Raspberry Pi Login]({{ site.url }}/images/loginpi.png)

Now to get familiar with some of the commands. You have logged in, and you are currently in the "pi" user's home directory. Type in `pwd` (print working directory) and hit enter to see the path of your current directory. Also try entering `ls` (list) to see the contents of the current directory. I have created a subdirectory here called `PiSession`, swap into it by typing `cd PiSession` (change directory), note that in Linux the case of you file and folder names matters! Use `pwd` to double check you are in the `PiSesion` directory. Now try creating your own folder with your name by typing `mkdir <yourname>` (make directory).

You can then swap into your own directory using `cd <yourname>`, and create files using `touch <filename>`. Try typing `touch info.txt`. You can then edit this file using a tool called [nano](http://www.nano-editor.org/ "GNU nano editor") `nano info.txt`. Enter some information, then hit ctrl-x to exit. You will be asked if you wish to save (y/n). If we want to swap back out of the directory we are in, we can use `cd ..` to move up one level, `cd ../..` to move up two levels, or `cd ../../..` to move up three levels, etc.
{% highlight bash %}
pi@gillypi ~ $ pwd
/home/pi
pi@gillypi ~ $ ls
Desktop    numpy-1.6.1         ocr_pi.png  Project-Mario  rtl-sdr
Documents  numpy-1.6.1.tar.gz  PiSession   python_games   Scratch
pi@gillypi ~ $ cd PiSession
pi@gillypi ~/PiSession $ pwd
/home/pi/PiSession
pi@gillypi ~/PiSession $ mkdir gilly
pi@gillypi ~/PiSession $ ls
gilly
pi@gillypi ~/PiSession $ cd gilly
pi@gillypi ~/PiSession/gilly $ pwd
/home/pi/PiSession/gilly
pi@gillypi ~/PiSession/gilly $ cd ../..
pi@gillypi ~ $ pwd
/home/pi
pi@gillypi ~ $ cd PiSession/gilly/
pi@gillypi ~/PiSession/gilly $ touch info.txt
pi@gillypi ~/PiSession/gilly $ ls
info.txt
pi@gillypi ~/PiSession/gilly $ 
{% endhighlight %}

Lets review what we have learned so far:

- Print the working directory `pwd`
- List the current directory's content `ls`
- Change into a directory `cd <dir>`
- Change up a directory `cd ..`
- Create an empty file `touch <filename>`
- Modify a file using nano `nano <filename>`

I have installed on my Raspberry Pi a program called [mpc](http://www.musicpd.org/clients/mpc/ "MPC page on MPD website"), Music Player Controller, which I have configured to play Triple J. Try typing `mpc play` or `mpc stop`, or even `mpc volume 80`. *Optional: You can find out more about mpc by typing `man mpc`, or indeed about most commands using the manual command, `man`.* I also have a command line Twitter program to monitor what Triple J is playing now, enter `t timeline` to see what's on.

Finally, try typing `python` to open the [Python](http://python.org/ "Python Homepage") [REPL](http://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop "REPL on Wikipedia"), which allows you to freely play with the Python language. Try typing in some maths:
{% highlight %}
pi@gillypi ~/PiSession/gilly $ python
Python 2.7.3 (default, Jan 13 2013, 11:20:46) 
[GCC 4.6.3] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 1+1
2
>>> print("hello world")
hello world
>>> import math
>>> math.cos(2*math.pi)
1.0
>>> 
{% endhighlight %}
Now use `ctrl-d` to log out of Python. Be careful, if you hit it twice you may log out of your SSH connection.

We will now try to do some basic output using Python. On the Pi, this is called GPIO, General Purpose Input/Output. *Advanced: Because using input and output could potentially be a security issue, we need to run the commands with privileged access, the same as running as administrator in windows. We do this using the `sudo` command, meaning super-user do.*

Type in `sudo python` (you may have to reenter your password).
{% highlight python %}
>>> import RPi.GPIO as GPIO # load the Raspberry Pi GPIO
>>> GPIO.setmode(GPIO.BCM) # Set which pin naming convention to use
>>> GPIO.setup(<pin>, GPIO.OUT) # Set <pin> as an output
>>> GPIO.output(<pin>, 1) # Set <pin> high, use 0 for low
{% endhighlight %}
I have connected LEDs to pins 9, 10, 22, 27, 17, 4, 3, 2 from left to right (sorry, the Pi pin numbering is funny, so so is my ordering).

Those wanting to go further should consider purchasing a Raspberry Pi. The units are very cheap, at $35 for a Raspberry Pi B+, or $45 for a Raspberry Pi 2. Of course, you will still need an SD card, keyboard, monitor, HDMI cable, USB micro cable, and a case, but many of these items you may already have lying around. The system itself is quite simple to learn, due to an abundance of material. Check out [Adafruit's guides](https://learn.adafruit.com/category/raspberry-pi "Learn Raspberry Pi at Adafruit").

That is about it for this session. We have seen how to connect to the Pi, how to navigate through the file system using command line, and how to run programs. These are the basic tools required to get started with the Pi. of course, if you have your own Pi, you can hook up a keyboard and monitor and connect directly. You can even connect a mouse and use a regular desktop environment. However, harnessing the power of the command-line unlocks the power of the Pi.