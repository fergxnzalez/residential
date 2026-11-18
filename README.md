# Marina Sky Residence – scroll-driven property tour

A single-page website where scrolling moves the camera through a luxury apartment in Dubai Marina.

- `index.html` – the whole site (HTML, CSS and JavaScript, no build step)
- `media/tour-desktop.mp4` – 1920×1080 tour used on landscape screens
- `media/mobile/` – portrait image sequence (15 fps, blended between frames) used on phones; mobile browsers often refuse to repaint a paused video while seeking, so phones draw these frames on a canvas instead
- `media/poster-*.jpg` – first frame shown while the video loads

The videos are encoded with a keyframe every 4–6 frames so the browser can seek smoothly as you scroll.

## Deploy

Static site: import the repository in Vercel with the default settings (Framework preset: **Other**, no build command, output directory `.`).

## Swapping in a real shoot

Replace the two MP4 files with the new walkthrough, keeping the same encoding settings:

```bash
ffmpeg -i walkthrough.mp4 -an -c:v libx264 -preset slow -crf 19 -g 6 -keyint_min 6 -bf 0 -sc_threshold 0 -movflags +faststart -pix_fmt yuv420p media/tour-desktop.mp4
```

For phones, regenerate the portrait frames (crop to 9:16 first if the shoot is landscape):

```bash
ffmpeg -i walkthrough.mp4 -vf "crop=ih*9/16:ih,fps=15,scale=540:960:flags=lanczos" -q:v 5 media/mobile/f_%03d.jpg
```

and set `SEQ.count` in `index.html` to the number of frames. Then adjust the `KEYS` array in `index.html` (the second at which each room's caption appears).

*Demo: the current video is AI-generated and the property is fictional.*
