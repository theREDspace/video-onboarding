# Video Concepts Overview

## Video File Format Breakdown

Container - wraps video, audio, subtitles, metadata, etc into a single file

![](./images/containers.png)

### Terms

Re-mux: Wrap a container's constituent parts (video, audio, etc) in a new container format. Parts (audio, video, subtitles, etc) may added/removed from the container as well. (Lossless)

Re-encode (aka transcode): Re-process video, audio, or both with new encoding settings (codec, bitrate, etc). (Lossy)

---

## Video Streaming Tech

### Progressive

* Single video file hosted on server
* Player downloads entire file

Pros
* Simple, broad compatibility

Cons
* Wasted data for content transferred but not watched
* Single video quality (bitrate), not ideal for many real-world network conditions
* No live streaming capability

### RTMP Streaming

Custom protocol for streaming video in chunks and variable qualities.
Superseded by HTTP-based streaming technologies

### HTTP Adaptive Bitrate Streaming

* Encodes source video to multiple different bitrates (renditions)
* Each rendition is split into small video 'segments' ~10 seconds long
* Standards: HLS, MPEG-DASH, MS Smooth Streaming, etc

Pros:
* Player will select best bitrate for the current network speed and can switch bitrates on the fly
* Builds buffer as-needed instead of continuously
* Use of HTTP for data transfer means cheaper hosting and no firewall issues
* No state management on server-side - adaptation logic is done client-side

Cons:
* Compatibility/Support

[Example ABS player (HLS, DASH, MSS) with bitrate graphs/stats](http://orange-opensource.github.io/hasplayer.js/1.11.0/samples/Dash-IF/index.html)

---

## HLS Specifics

![](./images/hls.png)

* Master playlist (.m3u8, aka manifest) includes URLs for bandwidth-specific playlist (.m3u8) files (aka rendition)
* Playlists for each rendition has URLs for video segments
* Video segments are in the .ts (transport stream) container format (newer versions also support mp4)
* Master playlist can also include various metadata and extra content such as subtitle streams/file, I-frame playlists, and more (what's possible depends on HLS version supported)
* Video files may be encrypted at rest and decryption keys can be provided in the playlists for the player to handle decryption
* Multiple master playlists can be generated (aka variants) for a single video and the client-side application can choose to play any one variant. This is often done to make different renditions available to different devices e.g. a mobile variant may contain lower minimum bandwidth renditions as compared to a variant meant for console.

HLS feature overview: <https://developer.apple.com/library/content/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html>

Example Playlist features: <https://developer.apple.com/library/content/technotes/tn2288/_index.html>

### Compatibility

Many devices/browsers don't support HLS natively:
![](./images/canhls.png)

Media Source Extensions (MSE) API can allow HLS format to be played in-browser and has broader support:
![](./images/canmse.png)

MSE allows JS scripts to take in the raw video data and re-mux it to a video container format that is supported by the current browser.
The project HLS.js handles this conversion via MSE and we can build video players on top of it supporting HLS playback on all major browsers.

---

## Extra Streaming Concepts

* Server Side Ad Insertion (SSAI):
  * AKA Stitched ads, dynamic ad insertion
  * Ads are not encoded into the video but are inserted later (often at manifest request-time) into the HLS playlists (uses HLS discontinuity tag to indicate separate content)
  * Allows for ads to be chosen based on current inventory and possibly targeted based on current user data
* VAST, VPAID, VMAP
  * Video advertising format standards developed by the IAB (Interactive Advertising Bureau)
  * We'll all be learning more about this
* Encrypted Media Extensions (EME)
  * Browser API allowing implementation of DRM (Digital Rights Management)

---

## Related Tech

* TV Everywhere (TVE) Authentication
  * Require a TV subscription to view video
  * Authentication (Auth-N): The user has a valid account with TV providers. TV providers are AKA as:
    * (US) MVPD - Multichannel video programming distributor
    * (CAN) BDU - Broadcast Distribution Undertaking
  * Authorization (Auth-Z): The user's TV subscription has access to a given channel.
  * Adobe Pass (aka Primetime authentication) is the main provider of TVE services, acting as the middleman between video apps and hundreds of MVPDs
  * Draw diagram

---

## References

* HLS overview: <https://developer.apple.com/library/content/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html>
* Example HLS Playlists: <https://developer.apple.com/library/content/technotes/tn2288/_index.html>
* MSE API Docs: <https://developer.mozilla.org/en-US/docs/Web/API/Media_Source_Extensions_API>
* hls.js: <https://github.com/video-dev/hls.js/tree/master>
* Adobe Pass Docs: <https://tve.helpdocsonline.com/home>
* Adaptive bitrate streaming overview: <https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming>
* Video Container Details: <https://en.wikipedia.org/wiki/Comparison_of_video_container_formats>