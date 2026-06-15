---
date: "2026-06-15T10:53:15+00:00"
type: "post"
title: "ffmpeg 指令筆記"
translationKey: "ffmpeg-command-notes"
categories:
  - "Notes 筆記"
  - "Computer Science 電腦科學"
---

- `-vn`：去除畫面
- `-an`：去除聲音
- `-c copy`：沿用原本的 codec
- `-c:v`：畫面 codec，如 libx264, libx265, libvpx
- `-c:a`：聲音 codec，如 aac, libopus, libmp3lame
- `-b:v`：畫面碼率
- `-b:a`：聲音碼率

# 重編碼
```sh
ffmpeg -i input.mp4 -c:v libvpx-vp9 -c:a libopus -b:a 6k output.webm
```

# 解析度調整
將影片解析度壓成 144p：
```sh
ffmpeg -i input.mp4 -vf "scale=-1:144" -c:a copy -b:a 16k output.mp4
```

# 裁切影片
```sh
ffmpeg -i input.mp4 -vf "crop=w:h:x:y" output.mp4

```

# 裁剪影片
```sh
ffmpeg -ss 00:00:00.00 -i input.mp4 -t 00:00:00.00 -c copy output.mp4
```
- `-ss`：開始點
- `-to`：結束點
- `-t`：時長

# 連接影片
```sh
ffmpeg -f concat -safe 0 -i input1.mp4 -i input2.mp4 -c copy output.mp4
```

# 加封面
```sh
ffmpeg -loop 1 -i cover.jpg -i input.mp4 -c:v libx264 -c:a copy output.mp4
```

# 旋轉影片
```sh
ffmpeg -i input.mp4 -vf "transpose=2" output.mp4
```
- 1 = 順時針 90 度
- 2 = 逆時針 90 度

# 加速影片
```sh
# 2 倍速：
ffmpeg -i input.mp4 -filter_complex "[0:v]setpts=0.5*PTS;[0:a]atempo=2.0[a]" -map "[v]" -map "[a]" output.mp4

# 8 倍速：
ffmpeg -i input.mp4 -filter_complex "[0:v]setpts=0.125*PTS[v];[0:a]atempo=8.0[a]" -map "[v]" -map "[a]" output.mp4

# 加速同時縮放影片：
ffmpeg -i input.mp4 -filter_complex "[0:v]setpts=0.125*PTS[v],scale=-1:144;[0:a]atempo=8.0[a]" -map "[v]" -map "[a]" output.mp4
```

# 轉成動圖
[轉成 WebP 動圖](https://wiwi.blog/docs/terminal/webp-animation)：
```sh
ffmpeg -i input.mp4 -vf "scale=360:-1,fps=12" -c:v libwebp_anim -loop 0 -compression_level 6 -quality 70 -an output.webp
```

# 浮水印
```sh
ffmpeg -i input.mp4 -i watermark.png -filter_complex "[0:v][1:v]overlay=10:10:enable='between(t,1,2)'[outv]" -map "[outv]" output.mp4
```
