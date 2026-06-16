---
date: "2026-06-15T10:53:15+00:00"
type: "post"
title: "ffmpeg Command Notes"
translationKey: "ffmpeg-command-notes"
categories:
  - "Notes 筆記"
  - "Computer Science 電腦科學"
---

- `-vn`: Remove video
- `-an`: Remove audio
- `-c copy`: Copy the original codec
- `-c:v`: Video codec, such as libx264, libx265, libvpx
- `-c:a`: Audio codec, such as aac, libopus, libmp3lame
- `-b:v`: Video bitrate
- `-b:a`: Audio bitrate

# Re-encode
```sh
ffmpeg -i input.mp4 -c:v libvpx-vp9 -c:a libopus -b:a 6k output.webm
```

# Changing resolution
Compress the video resolution to 144p:
```sh
ffmpeg -i input.mp4 -vf "scale=-1:144" -c:a copy -b:a 16k output.mp4
```

# Crop the video
```sh
ffmpeg -i input.mp4 -vf "crop=w:h:x:y" output.mp4

```

# Cut the video
```sh
ffmpeg -ss 00:00:00.00 -i input.mp4 -t 00:00:00.00 -c copy output.mp4
```
- `-ss`: Starting point
- `-to`: Ending point
- `-t`: Duration

# Concatenating videos
```sh
ffmpeg -f concat -safe 0 -i input1.mp4 -i input2.mp4 -c copy output.mp4
```

# Adding thumbnails
```sh
ffmpeg -loop 1 -i cover.jpg -i input.mp4 -c:v libx264 -c:a copy output.mp4
```

# Rotating the video
```sh
ffmpeg -i input.mp4 -vf "transpose=2" output.mp4
```
- 1 = 90 degrees clockwise
- 2 = 90 degrees counter-clockwise

# Speeding the video
```sh
# 2× speed:
ffmpeg -i input.mp4 -filter_complex "[0:v]setpts=0.5*PTS;[0:a]atempo=2.0[a]" -map "[v]" -map "[a]" output.mp4

# 8× speed:
ffmpeg -i input.mp4 -filter_complex "[0:v]setpts=0.125*PTS[v];[0:a]atempo=8.0[a]" -map "[v]" -map "[a]" output.mp4

# Speeding and scaling at the same time:
ffmpeg -i input.mp4 -filter_complex "[0:v]setpts=0.125*PTS[v],scale=-1:144;[0:a]atempo=8.0[a]" -map "[v]" -map "[a]" output.mp4
```

# Convert to an animated image
[Convert to animated WebP image](https://wiwi.blog/docs/terminal/webp-animation):
```sh
ffmpeg -i input.mp4 -vf "scale=360:-1,fps=12" -c:v libwebp_anim -loop 0 -compression_level 6 -quality 70 -an output.webp
```

# Adding watermarks
```sh
ffmpeg -i input.mp4 -i watermark.png -filter_complex "[0:v][1:v]overlay=10:10:enable='between(t,1,2)'[outv]" -map "[outv]" output.mp4
```
