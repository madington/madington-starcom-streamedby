# Madington Streamedby ad guide

## Intro

Streamedby is a video streaming solution for display ads developed by [Madington](https://madington.com/).

**Some of the features**:

- Dynamically plays the best possible quality stream for each browser using the most modern available codec (AV1, H265, webm etc)
- Automatically plays/pauses when the video is on/off screen enabling high quality playthrough statistics
- Limits total file size to be below 4MB in order to comply with Chromes' Heavy Ad Intervention program.

## Dev guide

Start by adding this script tag to your `html` file's `head` element:

`<script src="https://delivered-by-madington.com/streamedby/strcm/madington_streamedby.js"></script>`

### Simplest possible usage

Add a video element to your HTML:

`<video id="video" muted playsinline webkit-playsinline></video>`

\*_Note that the attributes `muted`, `playsinline` and `webkit-playsinline` must be present for the video to play as expected._

In your script, start the stream:

```
MadingtonStreamedby({
  videoElem: "#video", // The video element selector
  stream: "1613998784760-1dpqqx" // Stream ID provided by Madington
});
```

With this setup, the video will automatically start playing. The video element will behave as a normal video element. You can style it as you would normally do.

See `basic_demo.html` for a full working example.

### More advanced usage

Please see `enabler_demo.html` for a more advanced example of how to implement Streamedby.

Examples of possible options:

```
MadingtonStreamedby({
  videoElem: videoElem,
  stream: "1613998830989-ib4uxp",
  streamedbyCallback: streamedbyCallback, // Callback that will run when Streamedby is ready to play. Use this if you want to setup other things
  fallbackFunction: noAutoPlayFallback, // Function to init some kind of a fallback when autoplay is not possible
  timesToPlay: 4, // This will make the video loop and play 4 times
  shouldAutoPlay: true, //if set to false, you will have to call videoElem.play() to start the video playback
});
```

#### Full list of options

- `videoElem` (String or selector) (mandatory): can be a string selector (".video", "#myvideo") or a javascript reference to a video element.
- `stream` (String or Object)(mandatory): can be either a string ID or a stream object. To get the JSON object, go to https://ibv.streamedby.com/stream/${stream} (replace the string after "stream/" with your "ID").
  It can be useful to have the stream object in your code in order to get access to the poster images etc. It also saves the creative from an extra http request which it will have to do if you only input a string ID.
- `shouldAutoPlay` (boolean): Defaults to true. If set to false you will have to call videoElement.play() in order to start the video.
- `timesToPlay` (Integer): Use this if you want to override the number of times the video should play/loop.
- `shouldAutoLoop`(boolean): Defaults to true. If set to false the video will only play once. If set to true it will play as many times as possible within 30 seconds or as many times as set in the `timesToPlay` argument.
- `fallbackFunction`(function): When autoplay is not possible this function will be called if one is provided. Useful if you want to display a backup ad or a play button etc.
- `streamedbyCallback`(function): A function that will be called when Streamedby is ready to play. Use this to do additional setup

#### Polite loading

Since video's usually are quite heavy in kilobytes it is a good idea to use polite loading (i.e. only load the video until the parent page is fully loaded). See `enabler_demo.html` for an example of this using Google Doubleclick's `enabler.js`.

#### Player UI

In it's basic form Streamedby doesn't come with a built in player UI. It will, though, add and dynamically change stateful (and self-explanatory) classes to the body element depending on the state of the player. These classes can be used to create your own player UI. Please see `enabler_demo.html` for an example of this. The classes are:

```
"streamedby-no-autoplay",
"streamedby-play",
"streamedby-pause",
"streamedby-playing",
"streamedby-stalled",
"streamedby-waiting",
"streamedby-ended",
"streamedby-completed"
```
