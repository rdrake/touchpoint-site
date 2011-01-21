---
layout:  post
title:  Streaming Capture Devices (Webcams) to HTML5 Video Tag
---
# {{ page.title }}

Inspired by the work of __Louis__.

VLC can play almost every known video and audio format.  It is also capable of streaming and even transcoding this audio and video.  It can also stream a capture device such as a webcam.

Of course, if you don't have a webcam, that may be problematic.

## (Optional) Faking a Capture Device (Webcam)

Luckily somebody has created a loopback device for Video4Linux2 which allows us to fake a capture device.  You must install the v4l2loopback module, and load it.

	# modprobe v4l2loopback

You should now see capture devices under /dev.

	$ ls /dev/video*
	/dev/video  /dev/video0

Once the device exists, you can fake a video feed using Gstreamer with the following command:

	$ gst-launch-0.10 videotestsrc ! v4l2sink device=/dev/video0

## Streaming Capture Device

Simply modify the following command to fit your needs, then execute it.

	$ vlc -d v4l2:///dev/video0 --sout "#transcode{vcodec=theo,vb=800,scale=1,acodec=vorb,ab=128,channels=2,samplerate=44100}:http{dst=:8080/stream.ogv}

Change the input device to point to your capture device.  You can also change the transcoding settings to support other browsers (Ogg video is currently supported by Chrome, Opera, and Firefox).  You can also change the streaming settings to be on a different port and have a different filename.  This command also tells VLC to daemonize itself, or run in the background.
