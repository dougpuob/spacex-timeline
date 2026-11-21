# SpaceX 時間表

給 Douglas 訂閱的 SpaceX 發射與公司計畫日曆（iCal / RFC 5545）。時區標記為 Asia/Taipei，說明為繁體中文。

最後核對：2026-11-21（Asia/Taipei）
事件數：29

## 訂閱網址（Google Calendar「透過網址新增」）

```
https://raw.githubusercontent.com/dougpuob/spacex-timeline/main/spacex.ics
```

儲存庫：https://github.com/dougpuob/spacex-timeline

在 Google Calendar：其他日曆 → `+` → 透過網址新增 → 貼上上方網址。Google 有自己的重新整理節奏（常見是約 12–24 小時），檔案本身宣告 `REFRESH-INTERVAL` / `X-PUBLISHED-TTL` 為 6 小時。

## 為何不是 Gist

本次優先嘗試用 GitHub 連接器建立公開 gist `spacex.ics`，但失敗：

- 連接器：`user-GitHub-xai` 的 `create_gist`
- 帳號：`dougpuob`
- 錯誤：`POST https://api.github.com/gists: 403 Resource not accessible by integration`
- `cursor-github` 沒有 gist 工具

因此改為任務允許的備援：在同一套 GitHub 連接器建立公開儲存庫，並把 `spacex.ics` 放在 `main` 上。沒有 gist id。之後更新請直接改這個儲存庫裡的同一個檔案，不要再另建 gist（除非 gist 權限修復）。

## 如何更新同一個日曆檔

1. 在 `/workspace/spacex-timeline/` 研究並修改 `generate_ics.py` 的事件（只放有來源的事實，不要編造日期）。
2. 產生並驗證：

```bash
/workspace/spacex-timeline/.venv/bin/python /workspace/spacex-timeline/generate_ics.py
/workspace/spacex-timeline/.venv/bin/python -c "from pathlib import Path; from icalendar import Calendar; cal=Calendar.from_ical(Path('/workspace/spacex-timeline/spacex.ics').read_bytes()); print(len([c for c in cal.walk() if c.name=='VEVENT']))"
```

3. 用 `user-GitHub-xai` 的 `create_or_update_file` 覆寫 `dougpuob/spacex-timeline` 的 `spacex.ics`（branch: `main`）。更新既有檔案必須先用 `get_file_contents` 取得目前 blob SHA，再一併送出。
4. 用沒有 commit hash 的 raw 網址驗證（應永遠指向 `main` 最新版）：

```bash
curl -sSI https://raw.githubusercontent.com/dougpuob/spacex-timeline/main/spacex.ics
```

首次上傳後的 blob SHA（之後每次更新都會變，請重新讀取）：`d1971f11a11725181a7b1bf08682fba82c560e9d`

注意：經 GitHub API 上傳後，raw 檔可能是 LF 換行（本機產生的檔案是 CRLF）。Google Calendar 與 `icalendar` 都可解析；若要嚴格維持 CRLF，需改走會保留 `\r\n` 的 git 推送路徑。

## 2026-11-15 顯示內容更新

- 所有事件標題改為以 `SPCX ` 開頭（後已統一改為帶方括號的 `[SPCX] `），再接來源標籤，例如 `SPCX [官方] Falcon 9 USSF-317（范登堡）`。
- 每則事件說明都新增獨立的「參考來源」區塊，列出該事件用到的全部來源網址；原有的事件說明、可信度、最後核對日期都保留。
- 檢查後沒有需要合併的重複任務／里程碑（相近項目如 Flight 16 與 Ship 43 靜態點火、Artemis III／HLS 無人演示／Artemis IV、每週與每週兩次發射目標，都是不同事件，已保留）。

## 檔案約定

- 日曆名稱：`X-WR-CALNAME:SpaceX 時間表`
- 時區：`X-WR-TIMEZONE:Asia/Taipei`
- UID：穩定的 `slug@spacex-timeline`（更新事件時請沿用同一個 UID）
- 標題格式：`[SPCX] [官方]` / `[SPCX] [Elon X]` / `[SPCX] [媒體]` / `[SPCX] [媒體/傳聞]`（`[SPCX] ` 必須在來源標籤之前，且帶方括號）
- 說明含獨立的「參考來源」區塊（全部來源 URL），以及事件內容、可信度（確定 / 預計 / Elon 說法 / 傳聞）、最後核對日期
- 有精確時刻：UTC 計時事件（發射窗口用 `DTSTART`/`DTEND`）
- 只有日期：全日事件
- 只有季度或「年底」：全日放在估算日，標題與說明必須寫明模糊程度

## 未列入的事項

- SpaceX IPO 已於 2026-06-12 在 Nasdaq（SPCX）上市，故不作為「即將發生」事件。
- 例行 Starlink Falcon 9 發射若尚無官方或可靠時刻，未逐一列入，以免日曆被占位日期灌爆。
- Starship Flight 15 已於 2026-11-13 發射（11-14 完成飛船相關回收操作），故不列入即將事件。
