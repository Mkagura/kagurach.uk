---
title: "Revied: A 19.9CNY HDMI-USB Adapter"
date: 2023-04-07T14:56:06+08:00
draft: false
---



### The text is edited by Google Bard, there might be some problems.
---
# 0. Why am I buying this?
I am going to have another miniPC soon, and I want to run Windows on it, not a headless Linux machine. As you know, installing Windows without a monitor is not easy. I also cannot afford to buy a monitor.
One day, I was watching a video and I found out that there is something that can input HDMI and output to USB. This is probably what I am looking for.

# 1. Let's have a look at it
![Picture of it](https://i.ibb.co/gd3qQFp/photo-2023-04-10-22-02-23.jpg)
Well... quite simple, tings at this price always have few designs.  
It has a HDMI input and a USB output. Pretty simple, right?  
And there is also a menu. It is likely to have a metal cover. *Then we will know why it needs to be made of metal.*

# 2. How to use it
#### The menu gave us simple instructions for Windows:
1. Open VLC or PotPlayer.
2. Click on "Media" and select "Open Capture Device."
3. Select the "USB Video" device.
#### For linux, you need ffmpeg and v4l2, though there are other ways to use it
I referred to this [tutorial](https://www.mjt.me.uk/posts/fixing-missing-macrosilicon-ms2109/)  
use vim to write in ```/etc/udev/rules.d/91-hdmi-to-usb-ms2109.rules```
```bash 
# For HDMI-to-USB adaptor:
SUBSYSTEM=="usb", DRIVER=="snd-usb-audio", ATTRS{idProduct}=="2109", ATTRS{idVendor}=="534d", \
    RUN+="/bin/sh -c 'echo -n $kernel > /sys/bus/usb/drivers/snd-usb-audio/unbind'"

SUBSYSTEM=="usb", DRIVER=="snd-usb-audio", ATTRS{idProduct}=="2109", ATTRS{idVendor}=="534d", \
    ATTR{bInterfaceNumber}=="00", RUN+="/bin/sh -c 'sleep 1; echo -n $kernel > /sys/bus/usb/drivers/uvcvideo/bind'"
```
and then ```sudo udevadm control --reload-rules```  
To play the video, use  
 ```ffplay -f video4linux2 -framerate 50 -video_size 1920x1080 -input_format mjpeg /dev/video0```  
- the video0 can be video1 or ends with other numbers
- not all /dev/videox devices are avaliable but you can try

# 3. Feelings when using it
Problems:
- The quality is rather low when I tried it on Windows 11 (Surface Pro 6).
- It generates **too much** heat.
- It sometimes freezes and has problems outputting video, but reconnecting the USB side fixes it. *(This only happened on my Linux computer and the video devece also become unavaliable so I cannot say who caused this problem)*  

However, for a 19.9CNY adapter, it helped me to install Windows 11 and I would say it is worth buying if you need it.
