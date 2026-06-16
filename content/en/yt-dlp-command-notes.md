---
date: "2026-06-14T20:34:30+00:00"
type: "post"
title: "yt-dlp Command Notes"
translationKey: "yt-dlp-command-notes"
categories:
  - "Notes 筆記"
  - "Computer Science 電腦科學"
---

# Basic command
```sh
yt-dlp [param1] [param2] [param3] "URL 1" "URL 2" "URL 3"...
```

In addition to providing the URL for a single video, the URL of a playlist can also be used for batch downloading. This technique “may” work for platforms other than YouTube, but I haven’t tried it myself.

The `--output` parameter can be used to specify the output filename. If you don’t specify it, it will automatically fetch the video title and ID for you. I almost never specify the output filename by myself.

```sh
yt-dlp --output "%(title)s.%(ext)s" <URL>
```

This will remove the video ID from the output file name, leaving only the video title.

The `-P` parameter can be used to specify the output path. I currently rarely download in batches so I rarely change this setting.

# Search for available video formats
```sh
yt-dlp <URL> -F
```

If you use both `-F` and `-f`, followed by a format filter, it will list the available formats for the current video based on the filter you specified (or all available formats if no filter is specified), but it will not download the video.

# Custom codec priority
`yt-dlp` [thinks](https://github.com/yt-dlp/yt-dlp#sorting-formats) that the video codec AV1 is better than VP9 by default, but there’s a problem for AV1, which has a poor software decoding performance, resulting in a poor playback experience on some devices. Furthermore, I rarely need to access ultra-high resolution videos, so I’d like to lower the priority of AV1 encoding (at least to below H.265?):[^1]

```sh
yt-dlp <URL> -f (271/248)+251
```

This is a code snippet I hastily wrote before. However, this command only works if the target video provides one of the above-mentioned formats; otherwise, it will directly report an error.

In practice, if the video source is YouTube, the default audio format fetched is mostly the opus format numbered 251. Therefore, the audio format part can perhaps be directly replaced with `ba` or `bestaudio`.

```sh
yt-dlp <URL> -S "vcodec:vp9"
yt-dlp <URL> -f "bv*[vcodec!=av01]+ba/b"
```

# Custom download list
If the videos you want to download in batches are not already in the same playlist, you can also write the desired download list as a text file for `yt-dlp` to read:

```sh
yt-dlp --batch-file urls.txt
```

# Local preset options
- Linux/macOS path: `~/.config/yt-dlp/config/yt-dlp.conf`
- Windows path: `%APPDATA%\yt-dlp\config\yt-dlp.conf`; Or simply put `config.txt` under the same directory of `yt-dlp.exe`.

# Other options
- `--skip-download`: Skips the download process for videos. Usually used for fetching thumbnails or metadata.
- `--add-metadata`: Embeds the video’s metadata into the `synopsis` field of the video file, or the performer information in the audio file. `--write-description` outputs the description field as a text file.
- `--embed-thumbnail`: Attempts to embed the video thumbnail into the video file (provided the container format supports it). `--write-thumbnail` directly downloads the video thumbnail.
- `--embed-subs` and `--write-subs` options are used to download external subtitles. The `--sub-lang` option specifies the subtitle language. By default, external subtitles are not downloaded unless these two parameters are explicitly specified.
- `--list-subs`: Lists all available subtitles.
- `--keep-video`: Keep the orginal files after automatically merging the video and audio parts.
- `--split-chapters`: Split the output video into chapters automatically based on its chapter information.
- `-o -`: To output downloaded videos to `stdout`, generally used together with piping.
- `--cookies-from-browser <browser>`: If the downloaded target requires login for access, then this parameter is needed to feed the necessary cookies to `yt-dlp`. The name of a browser should follow this parameter, or you can also export the cookies manually.
- `--write-comments`: Downloads video comments.

# Other random thoughts
- Many years ago, I tried to fetch multi-part videos from Bilibili, but it could only fetch P1 from it. I haven’t tried this again at a later time.
- <span class="chide">Downloading Twitch live stream archives is very slow in practice, contacting the streamer directly for a download link might be way faster.</span>

[^1]: [Ivon](https://ivonblog.com/posts/yt-dlp-usage/) thinks that you can directly lock the downloaded video encoding to H.264, but for my own use, VP9 is also acceptable.
