# CLAUDE.md

## 專案
在本機執行的 Discord agent：指定頻道有人傳照片，就用 `claude -p` 判斷是不是食物、估成份／重量／熱量，存照片、寫進 Excel，再回覆上傳者結果和當日已攝取熱量。

## 開始任何工作前
先讀 `docs/architecture.md`，所有設計決定以它為準。需求原文在 `docs/requirements.md`。

## 四段防護
程式裡有四段防護，每一段都要用成對的註解清楚標出開始和結束，方便上課時單獨把某一段整段註解掉做對照：

```python
# ==== 防護 1：claude -p 的檔案範圍隔離　開始 ====
...
# ==== 防護 1 結束 ====
```

| # | 名稱 | 位置 |
|---|---|---|
| 1 | claude -p 的檔案範圍隔離 | `src/food_agent/analyzer.py` |
| 2 | 上傳者的文字只當資料 | `src/food_agent/analyzer.py` |
| 3 | 檔名清洗 | `src/food_agent/storage.py` |
| 4 | Excel 公式轉純文字 | `src/food_agent/storage.py` |

- 一開始四段**全部開啟**。
- 寫法：區塊前面先放「沒有防護」的預設值，區塊裡再換成安全的版本。這樣整段註解掉之後，程式照樣能跑。
- 防護的邏輯不要散在標記區塊之外，也不要把兩段防護混在同一個區塊裡。
- 改了程式、行號跑掉時，要一起更新 `README.md` 裡四段防護的行號表。

## Git
- **不可以** commit `.env`（裡面有 `DISCORD_TOKEN`）。
- **不可以** commit `DATA_DIR` 裡的東西（照片和 Excel 紀錄）。預設 `DATA_DIR` 是專案下的 `data/`；如果改成別的專案內路徑，要一併加進 `.gitignore`。
- commit 前用 `git status` 確認沒有上述檔案。
