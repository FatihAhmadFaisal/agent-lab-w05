# My lab evidence / 我的實作紀錄

Use a group code, not real names or student IDs in shared files. / 共用檔只寫組別代碼，不寫姓名或學號。

- Group code / 組別：411515024
- Tool / 工具：Antigravity (Google DeepMind)
- Route / 路線：individual 個人
- Tasks completed / 完成題目：A, B, D
- Material / 素材：NDHU classroom tasks 東華課堂版
- For original-pack work: task number, author/source link and version / 原版實作：題號、作者來源連結與版本：N/A (採用東華課堂版)
- My role and what I checked / 我的角色與實際檢查：
  擔任規劃與驗收者（Planner & Inspector）。
  1. 檢查 Agent 提出的分類計畫與資料邊界，確保不改動原始檔案且不連外。
  2. 檢查 Task A 的 12 個檔案是否完整複製至 output，比對重複檔與不同版本的處理是否正確。
  3. 實測 Task B 的 6 個邊界條件、隨機抽籤邏輯、中英文語系切換與歷史紀錄保護機制。
  4. 審查 Task D 刻意出錯的計畫，具體指出 5 項不可接受之處並撰寫退回報告。

## Scope and plan / 範圍與計畫

Allowed input and output folders / 可讀取與輸出的資料夾：
- Allowed Input:
  - `practice/01-club-files/input`
  - `practice/02-campus-picker/activities.json`
  - `practice/04-review/bad-plan.txt`
- Allowed Output:
  - `practice/01-club-files/output/`
  - `practice/02-campus-picker/output/index.html`
  - `practice/04-review/my-rejection.md`
  - `submission-template.md` & `evidence/`

What I asked for / 原始需求：
- Task A: 不改動 `input/` 原始檔案，將 12 個文字檔按性質複製歸類至適當子目錄，保留所有重複檔與草稿版本，產生 `manifest.json` 與 `report.md`。
- Task B: 依據 `activities.json` 製作離線可雙擊開啟之單頁工具「課間我想做什麼？」，支援地點、時間、強度三條件隨機篩選、顯示完整資訊、無符合條件清楚警示、保留最近5次成功紀錄（可清除）、篩選重設、中英文即時雙向切換，並加上免責聲明。
- Task D: 閱讀 `bad-plan.txt`，分析至少兩項嚴重問題並提出具體替代方案。

What I checked before execution / 動手前我檢查了什麼：
- 確認工作目錄受限於本題 `practice` 專案路徑，絕不跨至 Downloads 或系統目錄。
- 檢查 Agent 計畫是否落實「原檔不動、不刪除、不覆蓋、不連外、不偷改篩選條件」之安全邊界規範。

## Tests actually performed / 我真的做過的測試

| Test / 測試 | Expected / 預期 | Observed / 實際 | Evidence / 證據 |
|---|---|---|---|
| 1. 室外 / 15分鐘 / 中強度 (Task B 邊界篩選) | 顯示「沒有符合條件的活動」，不放寬條件 | 頁面精確顯示紅框警示「沒有符合條件的活動」，條件未被放寬，無不符項目被加入紀錄 | `practice/02-campus-picker/output/index.html` 畫面測試通過 (matched 長度為 0) |
| 2. 室外 / 30分鐘 / 中強度 (Task B 唯一匹配) | 每次抽籤只能是唯一符合的 A09 | 連續抽籤每次皆精準命中 A09「在合適位置快走」(30分鐘, outdoor, medium) | 實測 5 次皆為 A09，符合 `activities.json` 資料分布 |
| 3. 重設篩選 (Task B 狀態保護) | 回到不限/30/不限，歷史紀錄仍在 | 篩選條件即時還原為預設值 (不限/30分鐘/不限)，先前成功之抽籤紀錄完整保留無被抹除 | 實測點擊「重設篩選」按鈕後 history 陣列完好 |
| 4. Task A 檔案保留比對 | input 12 檔原檔未改，output 內 12 檔副本均在 | input 12 檔 Hash 與內容未變，output 分為 4 個資料夾共 12 檔，包含重複副本與 final/final2 | `manifest.json` 與 `report.md` 完整記載 12 筆對照與理由 |

## One revision / 一次修改

Before / 原來的情況：
第一版 (B v1) 只能使用滑鼠點擊按鈕，且在切換單選篩選條件時，使用者無法預先得知當前條件組合下有多少個活動可供抽選。

Request / 我提出的修改：
增加鍵盤無障礙支援（Enter / 空白鍵觸發「幫我選」、Esc 觸發「重設篩選」）並在按鈕呈現按鍵提示；同時新增即時「目前篩選符合候選項目」計算徽章（例如「符合 4 項活動」）。

After and retest / 修改後與重測結果：
修改後按下鍵盤 Enter 即可抽籤，按 Esc 即可重設，切換篩選條件即時更新候選筆數；各原創測項全部維持通過。修改成果獨立提交於 commit `B v2: add keyboard shortcuts and live matching count badge`。

New requirement or defect? / 新需求還是原規格未做到？：
新需求 (New requirement，為提升互動流暢度與無障礙操作之體驗優化)。

## One rejection / 一次退回

Which action I reject and why / 退回哪個動作、為什麼：
退回 `practice/04-review/bad-plan.txt` 中「整理整個 Downloads 資料夾」、「刪除重複檔案」、「逕自認定 final2 為最新定案版」、「缺失資料自行猜測填補」以及「完成後自動將成果對外公開」等 5 項危險動作。這會造成跨目錄非授權存取、不可逆之檔案遺失、虛構造假數據以及隱私外洩風險。

An acceptable alternative / 可以怎麼改：
改為限定工作範圍於指定目錄、保留所有原檔與副本並提供清單比對、不同版本皆保留並由人工開會定案、缺失值如實標記並列入問題報告、產出僅存於本機 output 由使用者人工驗收，未經核准不得連外公開。詳細退回內容記錄於 `practice/04-review/my-rejection.md`。

## Still unverified / 還沒驗證

What I cannot claim is complete / 哪些事不能說已完成：
1. 隨機抽選器於 6 次測試中雖符合邏輯，但不能在統計學上保證隨機數分佈具有長期之嚴格均勻性。
2. Task A 中 `proposal_final.txt` 與 `proposal_final2.txt` 兩份企畫草案究竟應採用室內版還是室外版，屬於社團行政裁決，尚未由幹部大會討論表決定案。
