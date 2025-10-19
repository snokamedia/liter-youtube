# \<lite-youtube\>

> A web component that renders YouTube embeds faster. The ShadowDom web component version of Paul's [lite-youtube-embed](https://github.com/paulirish/lite-youtube-embed).

## Features

- No dependencies; it's just a vanilla web component.
- It's fast yo.
- It's Shadow Dom encapsulated!
- It's responsive 16:9
- It's accessible via keyboard and will set ARIA via the `videotitle` attribute
- It's locale ready; you can set the `videoplay` to have a properly locale based label
- Set the `start` attribute to start at a particular place in a video
- You can set `autoload` to use Intersection Observer to load the iframe when scrolled into view.
- _new in v1.1_: Adds `nocookie` attr for use with use youtube-nocookie.com as iframe embed uri
- _new in v1.2_: Adds `playlistid` for playlist loading interface support
- _new in v1.3_: Adds `loading=lazy` to image placeholder for more perf with `posterloading` attr if you'd like to use eager
- _new in v1.4_: Adds `short` attr for enabling experimental YouTube Shorts mobile interaction support. See (example video)[https://www.youtube.com/watch?v=aw7CRQTuRfo] for details.
- _new in v1.5_: Adds support for nonce attribute via `window.liteYouTubeNonce` for CSP 2/3 support.
- _new in v1.6_: Adds `autoPause` for pausing videos scrolled off screen; adds `--lite-youtube-aspect-ratio` CSS custom property create custom aspect ratio videos; adds `--lite-youtube-frame-shadow-visible` CSS custom property to disable frame shadow (flat look); adds a named slot `image` that allows for setting custom poster image; adds `credentialless` for COEP
- _new in v1.7_: Adds support for styling the play button via `::part` (thank you [@Lukinoh](https://github.com/Lukinoh)!).
- _new in v1.8_: Component requires a named slot for the poster image. See basic usage.

## Important Changes

**As of version 1.8, this component no longer automatically fetches poster images from YouTube.** You **must** now provide an image via the `image` slot. This gives you more control over the appearance and performance of the component.  If no image is provided, an error will be logged to the console.


## Basic Usage

**You must provide a poster image using the `image` slot.**

```html
<lite-youtube videoid="guJLfqTFfIw">
  <img slot="image" src="your-poster-image.jpg" alt="Video Title">
</lite-youtube>
```

**Note:** The `alt` attribute on the image is important for accessibility.

## Basic Usage with Fallback Link

A fallback appears in any of the following circumstances:

1. Before the compontent is initialized
2. When JS is disabled (like `<noscript>`)
3. When JS fails or the lite-youtube script is not loaded/executed
4. When the browser doesn't support web components

```html
<lite-youtube videoid="guJLfqTFfIw">
  <img slot="image" src="your-poster-image.jpg" alt="Video Title">
  <a class="lite-youtube-fallback" href="https://www.youtube.com/watch?v=guJLfqTFfIw">Watch on YouTube: "Sample output of devtools-to-video cli tool"</a>
</lite-youtube>
```

Example CSS:

```css
.lite-youtube-fallback {
	aspect-ratio: 16 / 9; /* matches YouTube player */
	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: column;
	gap: 1em;
	padding: 1em;
	background-color: #000;
	color: #fff;
	text-decoration: none;
}

/* right-facing triangle "Play" icon */
.lite-youtube-fallback::before {
	display: block;
	content: '';
	border: solid transparent;
	border-width: 2em 0 2em 3em;
	border-left-color: red;
}

.lite-youtube-fallback:hover::before {
	border-left-color: #fff;
}

.lite-youtube-fallback:focus {
	outline: 2px solid red;
}
```

## Playlist Usage

Setting the YouTube playlistid allows the playlist interface to load on interaction. Note, this still requires a videoid for to load a placeholder thumbnail as YouTube does not return a thumbnail for playlists in the API.

```html
<lite-youtube
  videoid="VLrYOji75Vc"
  playlistid="PL-G5r6j4GptH5JTveoLTVqpp7w2oc27Q9"
>
  <img slot="image" src="your-playlist-poster.jpg" alt="Playlist Title">
</lite-youtube>
```

## Add Video Title

```html
<lite-youtube
  videotitle="This is a video title"
  videoid="guJLfqTFfIw"
>
  <img slot="image" src="your-video-poster.jpg" alt="Video Title">
</lite-youtube>
```

## Update interface for Locale

```html
<lite-youtube
  videoplay="Mirar"
  videotitle="Mis hijos se burlan de mi español"
  videoid="guJLfqTFfIw"
>
  <img slot="image" src="your-locale-poster.jpg" alt="Video Title">
</lite-youtube>
```

## Style It

Height and Width are responsive in the component.

```html
<style>
  .styleIt {
    width: 400px;
    margin: auto;
  }
</style>
<div class="styleIt">
  <lite-youtube videoid="guJLfqTFfIw">
    <img slot="image" src="your-styled-poster.jpg" alt="Video Title">
  </lite-youtube>
</div>
```

## Enable YouTube Shorts interaction on mobile

See [the example video](https://www.youtube.com/watch?v=aw7CRQTuRfo) of how this feature works for additional details.

```html
<lite-youtube videoid="vMImN9gghao" short>
    <img slot="image" src="your-shorts-poster.jpg" alt="Video Title">
</lite-youtube>
```

## AutoLoad with IntersectionObserver

Uses Intersection Observer if available to automatically load the YouTube iframe when scrolled into view.

```html
<lite-youtube videoid="guJLfqTFfIw" autoload>
  <img slot="image" src="your-autoload-poster.jpg" alt="Video Title">
</lite-youtube>
```

## Set a video start time

```html
<!-- Start at 5 seconds -->
<lite-youtube videoid="guJLfqTFfIw" videoStartAt="5">
  <img slot="image" src="your-start-time-poster.jpg" alt="Video Title">
</lite-youtube>
```

## Fine tune the poster quality for a video

> Note: This attribute is now mostly irrelevant since you are providing your own image.  It may have some effect on playlist thumbnails, but it is best to manage your own images.

```html
<lite-youtube
  videoid="guJLfqTFfIw"
  posterquality="maxresdefault"
>
  <img slot="image" src="your-quality-poster.jpg" alt="Video Title">
</lite-youtube>
```

## Use the named slot to set a custom poster image
```html
<lite-youtube videoid="guJLfqTFfIw">
  <img slot="image" src="my-poster-override.jpg" alt="Video Title">
</lite-youtube>
```

## Set custom aspect ratio
```html
<style>
  lite-youtube {
    --lite-youtube-aspect-ratio: 2 / 3;
  }
</style>
<lite-youtube videoid="guJLfqTFfIw">
  <img slot="image" src="your-aspect-ratio-poster.jpg" alt="Video Title">
</lite-youtube>
```

## Disable the frame shadow (flat look)
```html
<style>
  lite-youtube {
    /* No Shadow */
    --lite-youtube-frame-shadow-visible: no;
  }
</style>
<lite-youtube videoid="guJLfqTFfIw">
  <img slot="image" src="your-shadow-poster.jpg" alt="Video Title">
</lite-youtube>
```

## Customize the play button
```html
<style>
  lite-youtube::part(playButton) {
    /* You custom style */
  }
</style>
<lite-youtube videoid="guJLfqTFfIw">
    <img slot="image" src="your-play-button-poster.jpg" alt="Video Title">
</lite-youtube>
```

## Auto-Pause video when scrolled out of view

```html
 <lite-youtube videoid="VLrYOji75Vc" autopause>
    <img slot="image" src="your-autopause-poster.jpg" alt="Video Title">
 </lite-youtube>
```

## NoScript disable
As of v1.7.0, we inject into the lightdom a noscript for SEO help. This can conflict with server side rendered noscript injects. To disable, simply pass `disablenoscript` to the component:
```html
 <lite-youtube videoid="VLrYOji75Vc" disablenoscript>
    <img slot="image" src="your-noscript-poster.jpg" alt="Video Title">
 </lite-youtube>
```

## YouTube QueryParams

Use any [YouTube Embedded Players and Player Parameters](https://developers.google.com/youtube/player_parameters) you like.

> Note: the exception to this rule is the autoplay param; because of the nature of the performance loading and the inconsistency of usage, that parameter generally does not work. See [this comment](https://github.com/justinribeiro/lite-youtube/issues/66#issuecomment-1182110925) for details.

```html
<lite-youtube videoid="guJLfqTFfIw" params="controls=0&enablejsapi=1">
    <img slot="image" src="your-params-poster.jpg" alt="Video Title">
</lite-youtube>
```


## Attributes

The web component allows certain attributes to be give a little additional
flexibility.

| Name              | Description                                                                   | Default     |
|-------------------|-------------------------------------------------------------------------------|-------------|
| `videoid`         | The YouTube videoid                                                           | ``          |
| `playlistid`      | The YouTube playlistid; requires a videoid for thumbnail                      | ``          |
| `videotitle`      | The title of the video                                                        | `Video`     |
| `videoplay`       | The title of the play button (for translation)                                | `Play`      |
| `videoStartAt`    | Set the point at which the video should start, in seconds                     | `0`         |
| `posterquality`   | Set thumbnail poster quality (maxresdefault, sddefault, mqdefault, hqdefault) | `hqdefault` |
| `posterloading`   | Set img lazy load attr `loading` for poster image                             | `lazy`      |
| `nocookie`        | Use youtube-nocookie.com as iframe embed uri                                  | `false`     |
| `autoload`        | Use Intersection Observer to load iframe when scrolled into view              | `false`     |
| `autopause`       | Use video auto-pausing when scrolled out of view                              | `false`     |
| `short`           | Show 9:16 YouTube Shorts-style interaction on mobile devices                  | `false`     |
| `disablenoscript` | Disables `noscript` injector added to lightdom for search indexing            | `false`     |
| `params`          | Set YouTube query parameters                                                  | ``          |

## Events

The web component fires events to give the ability understand important lifecycle.

| Event Name                | Description                                      | Returns                             |
|---------------------------|--------------------------------------------------|-------------------------------------|
| `liteYoutubeIframeLoaded` | When the iframe is loaded, allowing us of JS API | `detail: { videoId: this.videoId }` |