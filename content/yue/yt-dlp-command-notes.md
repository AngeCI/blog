---
date: "2026-06-14T20:34:30+00:00"
type: "post"
title: "yt-dlp 指令筆記"
translationKey: "yt-dlp-command-notes"
categories:
  - "Notes 筆記"
  - "Computer Science 電腦科學"
---

# 基本指令
```sh
yt-dlp [param1] [param2] [param3] "URL 1" "URL 2" "URL 3"...
```

網址嘅部份除咗可以畀一條片嘅網址之外，亦都可以直接畀播放清單做批量下載。呢招對於 YouTube 以外嘅平台「或者」都用到，不過我自己就冇試過。

可以用 `--output` 參數指定輸出檔名，唔指定嘅話佢就會自己攞影片標題同影片 ID。我絕大多數時候都唔會特登去指定輸出檔名。

```sh
yt-dlp --output "%(title)s.%(ext)s" <URL>
```

咁樣就可以整走輸出檔名入面嘅影片 ID，得返個標題。

`-P` 參數可以指定輸出路徑，我目前好少批量下載所以好少掂呢樣。

# 查詢影片可用格式
```sh
yt-dlp <URL> -F
```

如果同時用 `-F` 同 `-f`，後面加埋格式過濾條件嘅話，佢就會根據你列出嘅過濾條件，列出呢條片可用嘅格式（如果冇加過濾條件嘅話就會列出所有可用格式），但唔執行下載影片嘅動作。

# 自定義編碼優先順序
`yt-dlp` [預設認為](https://github.com/yt-dlp/yt-dlp#sorting-formats)影像編碼 AV1 好過 VP9，但係 AV1 有個問題，佢軟解好渣，喺某啲設備嘅播放體驗好差，而且我好少會搞到極高清畫質，所以我希望將 AV1 編碼嘅優先程度降低少少（起碼降到 H.265 以下？）：[^1]

```sh
yt-dlp <URL> -f (271/248)+251
```

呢度係個之前求其寫嘅 code snippet。不過呢條指令用到嘅前提係，目標影片提供上述嘅呢幾個格，如果唔係會直接報錯。

實踐中，如果影片來源係 YouTube 嘅話，預設設置拎到嘅音頻格式，大多都係呢個編號係 251 嘅 opus 格式。所以音頻格式嘅部份或者可以直接用 `ba` 或者 `bestaudio` 替代。

```sh
yt-dlp <URL> -S "vcodec:vp9"
yt-dlp <URL> -f "bv*[vcodec!=av01]+ba/b"
```

# 自定義下載清單
如果你想要批量下載嘅影片唔喺同一個播放清單入面，你亦可以將你要嘅下載清單 save 做文字檔畀 `yt-dlp` 去睇：

```sh
yt-dlp --batch-file urls.txt
```

# 本地預設選項
- Linux/macOS 路徑：`~/.config/yt-dlp/config/yt-dlp.conf`
- Windows 路徑：`%APPDATA%\yt-dlp\config\yt-dlp.conf`。或者喺 `yt-dlp.exe` 所在嘅資料夾擺個 `config.txt`。

# 其他選項
- `--skip-download`：跳過下載影片。通常用嚟攞縮圖或 metadata。
- `--add-metadata`：將影片嘅 metadata 嵌入到影片檔嘅 `synopsis` 欄位，或者音訊檔嘅演出者資訊。而 `--write-description` 就會將把說明欄輸出成文字檔。
- `--embed-thumbnail`：嘗試將影片縮圖嵌入到影片檔度（但前提係容器格式支援）。而 `--write-thumbnail` 就會係直接下載影片縮圖。
- `--embed-subs` 同 `--write-subs` 用嚟下載外部字幕。用 `--sub-lang` 指定字幕語言。如果冇特別指定呢兩個參數嘅話，它預設係唔會下載外部字幕嘅。
- `--list-subs`：列出所有可用字幕。
- `--keep-video`：自動合併影音後，保留所有原始檔案。
- `--split-chapters`：根據影片嘅章節資訊自動分段影片。
- `-o -`：將下載嘅影片輸出到 `stdout`，一般需搭配管線符使用。
- `--cookies-from-browser <browser>`：如果下載嘅目標要登入先睇到嘅話，就需要利用呢個參數將需要嘅 cookie 餵畀 `yt-dlp`。呢個參數後面要跟一個瀏覽器嘅名，或者你手動匯出 cookie 都得。
- `--write-comments`：下載影片留言。

# 其他碎碎唸
- 好多年前我試過攞 Bilibili 上面嘅多 P 影片佢淨係攞到 P1，更後嚟嘅時間我就冇試過喇。
- <span class="chide">實試下載 Twitch 直播存檔很慢，直接聯絡片主拿下載連結可能會更快。</span>

[^1]: [Ivon](https://ivonblog.com/posts/yt-dlp-usage/) 認為，可以直接將下載嘅影片編碼鎖定喺 H.264，不過就我自己嘅用途而言，VP9 都係接受到嘅。
