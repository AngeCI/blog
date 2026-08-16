---
date: "2026-04-15T17:54:00+00:00"
lastmod: "2026-08-10T19:18:35+00:00"
type: "post"
title: "Rime 輸入方案開發筆記"
description: ""
translationsKey: "rime-schemata-developing-notes"
categories:
  - "Chinese IME 中文輸入法"
  - "Notes 筆記"
---

> [!WARNING] Notice
> 呢篇文章暫時仲未粵語化，暫時拎住[書面語版本](/blog/zh/rime-schemata-developing-notes/)頂住檔先。我遲啲有時間會更新返粵語版本！

> 呢版而家仲喺度**施工中**，敬請留意呢版嘅**持續更新**！

# 通用配置
通用配置文件名叫 `default.custom.yaml`。
```yaml
patch:
  "menu/page_size": 9	# 每頁候選字數目，預設好像是 5。
  "switcher/hotkeys":
    - "Control+grave"	# 有些人會禁用 F4 鍵，原因是會跟一些程式衝突。
```

# Windows 版（小狼毫）限定配置
Windows 版限定的配置文件名叫 `weasel.custom.yaml`。
```yaml
patch:
  "style/display_tray_icon": true	        # 強制顯示托盤圖標，不過在我的系統當中似乎預設就會顯示。
  "style/horizontal": true	                # 橫豎排，不過我個人反而比較習慣預設的豎排（原因下述）。
  "style/font_face": "Microsoft YaHei Mono"	# 字體，Windows 版預設的似乎是微軟雅黑。
  "style/font_point": 12	                # 字號，我個人覺得預設的字號有點大，所以把它調小了一點點。
  "style/color_scheme": "dark_temple"       # 配色，輸入法裏面本身就預裝了蠻多配色的，可以在設定面板中挑選，不過你想的話也可以自定義。
  "app_options/xxx.exe":
    ascii_mode: true	                    # 指定要強制禁用輸入法的程序（通常是遊戲的一些操作鍵會跟輸入法衝突）
```

# 實例
例如說我個人小時候用的是 Windows 內建的速成輸入法（據說和一些台灣人習慣的「ㄅ半」有點類似[^1]），比較習慣微軟速成輸入法用的那種佈局，我就可以透過以下參數控制 Rime 輸入方案的外觀：
```yaml
patch:
  "menu/page_size": 9	# 將每頁候選字數目設為 9 個。
```

<!--
## Caps Lock 屎山
Rime 對於 Caps Lock 的處理簡直是個屎山。在 [GitHub Issues]() 上，可以看出不同的用戶對於 Caps Lock 按鍵的功能都有不同（且有可能相互衝突）的需求。
-->

## Shift 鍵頂字上屏
```yaml
ascii_composer/good_old_caps_lock: true
ascii_composer/switch_key:
  Caps_Lock: noop
  Shift_L: commit_code
  Shift_R: inline_ascii
  Control_L: clear
  Control_R: commit_text
```

## 空格鍵換頁
```yaml
key_binder:
  bindings:
    - {when: has_menu, accept: space, send: Next}
```

## 碼唯一自動上屏
```yaml
speller:
  auto_select: true  # 碼唯一自動上屏
  max_code_length: 4 # 第五碼頂字上屏
```

## 空碼按空格清空
在默認情況下，輸入無效的編碼後按空格，會直接將輸入的鍵位上屏。這種特性在形碼輸入上通常有點礙事，可以如下方式修改：
```yaml
key_binder:
  bindings:
    - {when: composing, accept: space, send: Escape} # 空碼按空格清空
```

# 參考
- [Rime 官方文檔](https://github.com/rime/home/wiki/RimeWithSchemata)

[^1]: 不同的是，微軟後來推出的「新速成輸入法」，對於習慣舊版的人來說其實很難用。
