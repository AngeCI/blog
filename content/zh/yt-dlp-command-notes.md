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

網址的部份除了給單部影片的網址之外，也可以直接給播放清單做批量下載。這招對於 YouTube 以外的平台「也許」也能用，不過我自己就沒試過。

可以使用 `--output` 參數指定輸出檔名，不指定的話它就會自己抓影片標題和影片 ID。我絕大多數時候都不會去特別指定輸出檔名。

```sh
yt-dlp --output "%(title)s.%(ext)s" <URL>
```

如此就可以把輸出檔名中的影片 ID 去掉，只留下影片標題。

`-P` 參數可以指定輸出路徑，我目前很少批量下載所以很少動這一項。

# 查詢影片可用格式
```sh
yt-dlp <URL> -F
```

如果同時使用 `-F` 和 `-f`，後面加上格式過濾條件的話，那它就會根據你列出的過濾條件，當前影片的可用格式（如果不加過濾條件的話就會列出所有可用格式），但不執行下載影片的動作。

# 自定義編碼優先順序
`yt-dlp` [預設認為](https://github.com/yt-dlp/yt-dlp#sorting-formats)影像編碼 AV1 優於 VP9，但是 AV1 有個問題，它的軟解很爛，在某些設備上的播放體驗很差，而且我很少會弄到極高清畫質，所以我希望把 AV1 編碼的優先程度降低一些（至少降到 H.265 以下？）：[^1]

```sh
yt-dlp <URL> -f 271/248+251
```

這是一個之前草草寫的 code snippet。不過這條指令能用前提是，目標影片提供上述的這幾個格式，否則會直接報錯。

實踐中，若影片來源是 YouTube 的話，預設設置抓到的音頻格式，大多也是這個編號為 251 的 opus 格式。所以音頻格式的部份也許可以直接以 `ba` 或者 `bestaudio` 替代。

# 自定義下載清單
如果你想要批量下載的影片不在同一個播放清單，你也可以將你要的下載清單存成文字檔讓 `yt-dlp` 去讀：

```sh
yt-dlp --batch-file urls.txt
```

# 本地預設選項
- Linux/macOS 路徑：`~/.config/yt-dlp/config/yt-dlp.conf`
- Windows 路徑：`C:\Users\使用者名稱\AppData\Roaming\yt-dlp\config\yt-dlp.conf`。或者在 `yt-dlp.exe` 所在的資料夾放置一個 `config.txt`。

# 其他選項
- `--skip-download`：跳過下載影片。通常用於擷取縮圖或 metadata。
- `--add-metadata`：把影片的 metadata 嵌入到影片檔的 `synopsis` 欄位，或者音訊檔的演出者資訊。而 `--write-description` 則是把說明欄輸出成文字檔。
- `--embed-thumbnail`：嘗試把影片縮圖嵌入到影片檔內（但前提是容器格式支援）。而 `--write-thumbnail` 則是直接下載影片縮圖。
- `--embed-subs` 和 `--write-subs` 用於下載外部字幕。用 `--sub-lang` 指定字幕語言。若不特別指定這兩個參數的話，它預設是不會下載外部字幕的。
- `--list-subs`：列出所有可用字幕。
- `--keep-video`：自動合併影音後，保留所有原始檔案。
- `--split-chapters`：根據影片的章節資訊自動分段影片。
- `-o -`：將下載的影片輸出到 `stdout`，一般需搭配管線符使用。
- `--cookies-from-browser <browser>`：如果下載的目標需要等入才能存取的話，就需要利用這個參數將需要的 cookie 餵給 `yt-dlp`。這個參數後面要跟一個瀏覽器名稱，或者你手動匯出 cookie 也可以。
- `--write-comments`：下載影片留言。

# 其他碎碎唸
- 很多年前我試着抓 Bilibili 的多 P 影片它只能抓到 P1，更後來的時間我就沒試過了。
- <span class="chide">實試下載 Twitch 直播存檔很慢，直接聯絡片主拿下載連結可能會更快。</span>

[^1]: [Ivon](https://ivonblog.com/posts/yt-dlp-usage/) 認為，可以直接將下載的影片編碼鎖定在 H.264，不過就我自己的用途而言，VP9 也是可以接受的。
