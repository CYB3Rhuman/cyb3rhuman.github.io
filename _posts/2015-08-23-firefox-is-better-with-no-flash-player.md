---
layout: post
title: Firefox is better with no Flash Player
---

Recently Mozilla Firefox started to block Flash Player plugin by default. And many users were worried that without it web won't be so fun and shiny anymore. Let's destroy this myth!

Flash Player is widely used for audio and video playback on the web. But it is not the only option nowadays: modern browsers can play audio and video without Flash Player.

What else Flash Player may be used for?

- **Streaming video.** Most of TV and sexy webcam streaming sites will not work, but YouTube supports video streams with Media Source Extensions enabled and Twitch [has plans on revealing a new HTML5 player](http://blog.twitch.tv/2015/07/video-player-controls-now-in-html/) soon.

- **Webcam and microphone access.** Chatroulette and Omegle will not work either. The technology that should be used nowadays is WebRTC. If you just want to chat with friends, not with strangers, you can use Firefox Hello.

- **Text chats.** Not only video- and voice-chats use Flash. Some text chats do. This is really retarded. There are so many chat implementations with AJAX and some new ones with WebSockets, why use Flash here?

- **File uploaders.** Back in those days browsers couldn't select multiple files or upload really big file to a server. Not anymore. All Flash-based file uploaders must die.

- **Vector animation.** Yes, you will have hard time playing your favorite swf-cartoons with Shumway. Try to find them rendered on YouTube.

- **Games.** Tons of them. And again Shumway doesn't usually help much.

- **Banners.** Annoying animated advertisement. Must die.

- **Clipboard access and keylogging.** Some nasty websites do steal your clipboard contents. This is a feature, not a bug.

- **Malware.** Flash Player is full of vulnerabilities, so it's a preferred technology for embedding malware in web pages.

If you are okay with that, just remove Flash Player plugin from your computer and enjoy clean and fast web.

## Testing if your multimedia configuration is alright

If you can't play some media on the web, probably your browser is misconfigured. There are some tests to perform before contacting website's tech support:

- **Audio.** If you are experiencing troubles with audio playback, check for [supported audio formats](http://hpr.dogphilosophy.net/test/index.php). All formats except FLAC should be playable.

- **Video.** If you are experiencing troubles with video playback, check for [supported video formats](http://www.quirksmode.org/html5/tests/video.html#link2). All three should be played. Note that flv videos should not work.

- **MSE.** If you are experiencing troubles with video streams, check for MSE support on [YouTube HTML5 test page](https://www.youtube.com/html5). Note that HLS streams are only supported on Apple devices.

- **WebRTC.** If you have problems with your webcam or microphone, go to [WebRTC Troubleshooter](https://test.webrtc.org/) and try to solve them there.

## Configuring Firefox

First of all, ensure that your version of Firefox is up-to-date. If you are running Debian-based distro (e.g. Ubuntu, Mint, etc.) ensure that you have `gstreamer1.0-libav` installed.

To get video acceleration working, you may want to install `gstreamer1.0-vaapi` (I don't know how to check if it's really working or not).

You don't have to remove Flash Player from your system (however it's strongly recommended), you may just disable it in `about:addons` ("Plugins" tab).

That should be enough for MP4 and MP3 playback without Flash Player. Now most of websites with video and music will work.

If it doesn't work, check for `media.gstreamer.enabled` in `about:config`, it should be set to `true`.

### Enabling Media Source Extensions

For YouTube video streaming and advanced player features MSE is used. You may follow [this guide](http://www.linuxveda.com/2015/04/02/enable-mse-native-html5-support-firefox-linux/) to enable it.

In most cases just setting these values in `about:config` is enough:

```
media.mediasource.enabled               true
media.mediasource.mp4.enabled           true
media.mediasource.webm.enabled          true
media.fragmented-mp4.enabled            true
media.fragmented-mp4.exposed            true
media.fragmented-mp4.ffmpeg.enabled     true
media.fragmented-mp4.gmp.enabled        true
media.fragmented-mp4.use-blank-decoder  false
```

### Enabling WebRTC

In most cases WebRTC should work out of the box. If it doesn't, check that `media.peerconnection.enabled` in `about:config` is set to `true`.

Check that your mic and webcam work. You may try to record a video with Cheese. If the picture and sound are clear, your webcam and mic are set up correctly.

### Still need to play an .swf?

If you need to play Flash animation, try [Shumway](http://www.areweflashyet.com/shumway/) — a Flash VM and runtime written in JavaScript developed by Mozilla.

It will certainly not play any SWF just like Flash Player does, but sometimes it helps.

## Advanced tuning

For more advanced Firefox tuning tips (e.g. enable hardware acceleration with OpenGL for web pages, moving cache from hard drive to RAM, enabling tracking protection), refer to [this article](https://tlhp.cf/firefox-tuning/).

