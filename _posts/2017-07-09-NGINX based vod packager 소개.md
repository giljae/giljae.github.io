---
title:  "NGINX based vod packager 소개"
tags: Nginx, vod packager
---

![image](https://user-images.githubusercontent.com/111643/115819090-1f4a8280-a439-11eb-9995-8c17c715d6ef.png)

일반적으로 NGINX Level에서는 HLS,DASH Streaming을 지원해준다. 이 부분은 굳이 NGINX가 아니더라도 Tomcat container와 같은 Application Server에서는 일반적으로 지원해주고 있다. (HTTP requst range 기반으로 작동되므로…)

하지만, Media Streaming을 위해서는 기본적인 Streamer 기능 외에 필요한 부분들이 있다. NGINX의 경우에는 이런 부분들을 3rd party 개발자분들이 module을 제작하여 Github를 통해 공개하고 있다.

NGINX에서 가장 유명한 VOD module은 nginx-rtmp-module이지만, 본 글에서는 nginx-vod-module에 대해서 다루고자 한다.

# Features
* On-the-fly repackaging of MP4 files to DASH, HDS, HLS, MSS
* Working modes:
* Local: serve locally accessible files (local disk/NFS mounted)
* Remote: serve files accessible via HTTP using range requests
* Mapped: serve files according to a specification encoded in JSON format. The JSON can pulled from a remote server, or read from a local file
* Adaptive bitrate support
* Playlist support — mapped mode only
* Simulated live support (generating live stream from MP4 files) — mapped mode only
* Fallback support for file not found in local/mapped modes (useful in multi-datacenter environments)
* Video codecs: H264, H265 (DASH/HLS), VP9 (DASH)
* Audio codecs: AAC, MP3 (HLS/HDS/MSS), AC-3 (DASH/HLS), E-AC-3 (DASH/HLS), OPUS (DASH)
* Playback rate change — 0.5x up to 2x (requires libelcodec and libavfilter)
* Support for variable segment lengths — enabling the player to select the optimal bitrate fast, without the overhead of short segments for the whole duration of the video
* Clipping of MP4 files for progressive download playback
* Thumbnail capture (requires libavcodec)
* Decryption of CENC-encrypted MP4 files (it is possible to create such files with MP4Box)
* DASH: common encryption (CENC) support
* MSS: PlayReady encryption support
* HLS: Generation of I-frames playlist (EXT-X-I-FRAMES-ONLY)
* HLS: support for AES-128 / SAMPLE-AES encryption

생각외로 많은 기능을 제공해주고 있다. 물론 제약도 존재한다.

# Limitations
* Track selection and playback rate change are not spported in progressive download
* I-frames playlist generation is not supported when encryption is enabled
* Tested on Linux only

