- [Checking for support](#checking-for-support)
  - [video.canPlayType](#videocanplaytype)
  - [MediaSource.isTypeSupported](#mediasourceistypesupported)
- [HDR support](#hdr-support)
  - [Color Depth (CSSOM View Module, Editors Draft, 9 June 2021)](#color-depth-cssom-view-module-editors-draft-9-june-2021)
  - [Color Gamut - MatchMedia](#color-gamut---matchmedia)
  - [Use with MediaCapabilities API](#use-with-mediacapabilities-api)

## Checking for support

Testpage: https://cconcolato.github.io/media-mime-support/
Reference: https://developer.mozilla.org/en-US/docs/Web/Media/Formats/codecs_parameter

### video.canPlayType

|        Description        |                    Argument                     |
| :-----------------------: | :---------------------------------------------: |
|   Dolby Digital (AC-3)    | `video.canPlayType('audio/mp4; codecs="ac-3"')` |
| Dolby Digital Plus (EC-3) | `video.canPlayType('audio/mp4; codecs="ec-3"')` |


### MediaSource.isTypeSupported

|                                 Description                                  |              Argument              |
| :--------------------------------------------------------------------------: | :--------------------------------: |
|    An MPEG-4 file containing AVC (H.264) video, Main Profile, Level 4.2.     | `video/mp4; codecs="avc1.4d0002a"` |
|                    An MPEG-4 file containing AAC-LC audio                    |  `audio/mp4; codecs="mp4a.40.2"`   |
| An MPEG-4 file containing AV1 video, Main Profile, 8-bit color, all defaults | `video/mp4; codecs="av01.0.15M.8"` |

## HDR support

### Color Depth (CSSOM View Module, Editors Draft, 9 June 2021)

Previously `colorDepth` and `pixelDepth` both were set to always return `24` for compatibility reasons.

Check `window.screen.colorDepth` for monitor support of HDR content.

> The colorDepth and pixelDepth attributes should return the number of bits allocated to colors for a pixel in the output device, excluding the alpha channel. If the user agent is not able to return the number of bits used by the output device, it should return the closest estimation such as, for example, the number of bits used by the frame buffer sent to the display or any internal representation that would be the closest to the value the output device would use. The user agent must return a value for these attributes at least equal to the value of color media query multiplied by three. If the different color components are not represented with the same number of bits, the returned value may be greater than three times color media query. If the user agent does not know the color depth or does not want to return it for privacy considerations, it should return 24.

Chrome implements support for the above in [59+](https://www.chromestatus.com/feature/5743005361242112).

### Color Gamut - MatchMedia

[Live Official Sample](https://googlechrome.github.io/samples/media/color-gamut-media-query.html)

JavaScript reproduced here.

```
if (window.matchMedia("(color-gamut: srgb)").matches) {
  console.log(`Screen supports approximately the sRGB gamut or more.`);
}

if (window.matchMedia("(color-gamut: p3)").matches) {
  console.log(`Screen supports approximately the gamut specified 
      by the DCI P3 Color Space or more.`);
}

if (window.matchMedia("(color-gamut: rec2020)").matches) {
  console.log(`Screen supports approximately the gamut specified
      by the ITU-R Recommendation BT.2020 Color Space or more.`);
}
```

### Use with MediaCapabilities API

[Specification](https://www.w3.org/TR/2021/WD-media-capabilities-20210604/) Type: "HdrMetadataType" set on `VideoConfiguration.hdrMetadataType` property.

```
enum HdrMetadataType {
  "smpteSt2086",
  "smpteSt2094-10",
  "smpteSt2094-40"
};
```
