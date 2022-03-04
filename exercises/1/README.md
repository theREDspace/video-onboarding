# Achieving background fullscreen video playback

This was a common trend, getting a large background video playing on your website.
You can even see it in use on the REDspace website itself.

I've added the autoplay attribute to the `<video>` element, and the source is pointed at the progressive MP4 served from the assets folder.

However, there is a problem, the video doesn't play.

Why doesn't the video play in this exercise?

## Once you've got it playing

Adds the `controls` attribute to the `<video>` element.

Throttle your network connection to Slow 3G and disable your cache so you can watch the network requests for the video.

Pay attention to the controls as the video is loading, you may notice something different about this video than ones you are used to viewing. 

What is it?

How would you fix it?