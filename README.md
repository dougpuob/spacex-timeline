# SpaceX 時間表

SpaceX 近期發射與中長期計畫的可訂閱日曆（iCal）。說明為繁體中文，時區為 Asia/Taipei。

## 用 Google Calendar 訂閱

在 Google 日曆選擇「其他日曆」→「透過網址新增」，貼上：

```
https://raw.githubusercontent.com/dougpuob/spacex-timeline/main/spacex.ics
```

此網址永遠指向 `main` 分支上的最新檔案（不含 commit hash）。

## 標籤與可信度

- `[官方]`：SpaceX、NASA 等官方頁面或公司公開說明（例如財報會）
- `[Elon X]`：Elon Musk 的 X 貼文
- `[媒體]`／`[媒體/傳聞]`：Next Spaceflight、NASASpaceflight 等彙整或媒體報導

每則事件說明都有來源網址、可信度（確定／預計／Elon 說法／傳聞），以及最後核對日期。只有季度或「年底」的目標，會放在估算日期，並在標題與說明標明並非鎖定發射日。

發射窗口與精確時刻以 UTC 記錄；只有日期的項目為全日事件。

## 更新這個檔案

修改並驗證本機的 `spacex.ics` 後，用 GitHub API 或 git 覆寫 `main` 上的同一個 `spacex.ics`（更新既有檔案需提供目前 blob SHA）。不要改檔名，也不要改成帶 commit hash 的 raw 網址，否則訂閱會失效。
