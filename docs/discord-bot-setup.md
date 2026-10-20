# Discord Bot 設定步驟（Windows）

這份文件帶你完成：
1. 在 Discord Developer Portal 建立 application 和 bot
2. 打開 Message Content Intent
3. 拿到 bot token，放進 `.env`
4. 設定權限，把 bot 邀進你的伺服器

開始之前，請先完成 `docs/discord-server-setup.md`（要有自己的伺服器）。

> Developer Portal 只有英文介面，下面的按鈕名稱都是英文。

---

## ⚠️ 先讀這段：bot token 是密碼

bot token 等於這個 bot 的**帳號加密碼**。拿到 token 的人可以冒充你的 bot：讀你伺服器裡的訊息、用 bot 的名義亂發訊息。

- ❌ **絕對不能**貼給任何人，包括同學和助教。
- ❌ **絕對不能**貼進和 Claude 的對話（包括這個對話）。Claude 不需要看到 token，程式會自己從 `.env` 讀。
- ❌ **絕對不能** commit 進 git、上傳到 GitHub。就算之後刪掉，git 歷史裡還是找得到。
- ❌ 不要截圖，也不要貼在 LINE、Discord、筆記軟體裡。
- ✅ **唯一**放 token 的地方是你電腦上專案資料夾裡的 `.env` 檔案。

**不小心外流了怎麼辦**：馬上照下面「三、拿到 bot token」的步驟按 **Reset Token**。舊的 token 會立刻失效，再把新的 token 換進 `.env` 就好。

---

## 一、建立 application

1. 用瀏覽器打開 <https://discord.com/developers/applications>，用你的 Discord 帳號登入。
2. 點畫面**右上角**藍色的 **「New Application」**。
3. **Name** 欄位輸入 bot 的名字，例如 `Food Logger`。這就是之後大家在頻道裡看到的 bot 名稱。
4. 勾選下面的同意條款方框。
5. 點 **「Create」**。如果跳出「我不是機器人」的驗證，照指示完成。
6. 你會進到這個 application 的 **General Information** 頁面。左邊有一排選單，後面的步驟都從這裡點。

---

## 二、打開 Message Content Intent（一定要做）

1. 點左邊選單的 **「Bot」**。
2. 頁面往下捲，找到 **「Privileged Gateway Intents」** 這一區。
3. 找到 **「MESSAGE CONTENT INTENT」**，把右邊的開關**打開**（變成藍色）。
   - 上面的 Presence Intent、Server Members Intent **不用開**。
4. 頁面最下方會跳出一條提示，點 **「Save Changes」**。**沒按儲存就不算數。**

**為什麼一定要開？**
Discord 把訊息內容當成隱私資料。bot 預設只會知道「有人在某頻道發了一則訊息」，**看不到內容，附件也看不到**。agent 要拿到同學傳的照片，就必須開這個 intent。沒開的話程式照樣能連上 Discord，但每則訊息看起來都是空的，傳照片進來它完全沒反應，而且不會跳出任何錯誤，很難找原因。

---

## 三、拿到 bot token

1. 還在左邊的 **「Bot」** 頁面，往上捲回頁面頂端。
2. 找到 **「TOKEN」** 這一區，點 **「Reset Token」**。
   - 新建立的 bot 第一次也是按這個按鈕，Discord 只會在按下的當下顯示一次 token。
3. 跳出確認視窗，點 **「Yes, do it!」**。
4. 如果你的 Discord 帳號有開兩步驟驗證，這時要輸入驗證碼。
5. 畫面會出現一長串文字，點旁邊的 **「Copy」**。
6. **馬上**照下面的步驟存進 `.env`，不要貼到其他地方。

### 存進 `.env`

1. 打開檔案總管，進到專案資料夾（例如 `C:\Users\Grace\1020`）。
2. 如果已經有 `.env` 檔，在它上面按右鍵 → **「開啟檔案」**（或「開啟方式」）→ 選 **記事本**。
   如果還沒有：打開記事本，最後存檔時看下面第 5 步。
3. 加一行（等號後面直接貼上，按 **Ctrl + V**，不要加引號、不要有空格）：
   ```
   DISCORD_TOKEN=這裡貼上剛剛複製的token
   ```
4. 存檔（**Ctrl + S**）。
5. 如果是新檔案，在「另存新檔」視窗：
   - **存檔類型**選 **「所有檔案 (\*.\*)」**
   - **檔案名稱**輸入 `.env`
   - 存到專案資料夾裡

   這樣才不會變成 `.env.txt`。
6. 確認 git 不會收進這個檔案：在專案資料夾的終端機打 `git status`，清單裡**不應該**出現 `.env`。

---

## 四、建議：把 bot 設成私人（只有你能邀請）

不做也能用，但做了之後，別人就算知道 bot 的 ID，也不能把它邀進他們的伺服器。

1. 點左邊選單的 **「Installation」**。
2. 找到 **「Install Link」** 下拉選單，改成 **「None」**，然後點 **「Save Changes」**。
   （要先做這一步，不然下一步會儲存失敗。）
3. 點左邊選單的 **「Bot」**，找到 **「PUBLIC BOT」**，把開關**關掉**，再點 **「Save Changes」**。

---

## 五、設定權限並把 bot 邀進你的伺服器

### 1. 產生邀請網址

1. 點左邊選單的 **「OAuth2」**。
2. 往下捲到 **「OAuth2 URL Generator」**。
3. **SCOPES** 的一堆方框裡，只勾 **`bot`**。
4. 勾完下面會多出 **「BOT PERMISSIONS」** 一區，**只勾這四個**：

   | 權限（在哪一欄） | 為什麼需要 |
   |---|---|
   | **View Channels**（GENERAL PERMISSIONS） | 看得到 `# food-log` 頻道 |
   | **Send Messages**（TEXT PERMISSIONS） | 回覆結果 |
   | **Read Message History**（TEXT PERMISSIONS） | 能「回覆」某一則照片訊息 |
   | **Add Reactions**（TEXT PERMISSIONS） | 加上 ⏳ ✅ ❌ ⚠️ |

   > **不要勾 Administrator**。只給剛好夠用的權限，就算 bot 出問題，能造成的傷害也有限。

5. 如果下面有 **Integration Type**，選 **「Guild Install」**。
6. 頁面最下面的 **GENERATED URL** 會出現一條網址，點右邊的 **「Copy」**。

### 2. 用網址邀請 bot

1. 開一個新的瀏覽器分頁，貼上網址，按 Enter。
2. **「Add to Server」**（新增至伺服器）下拉選單選你的伺服器，例如 `食物記錄測試`。
   - 選單裡只會出現你有「管理伺服器」權限的伺服器，你自己建的會在裡面。
3. 點 **「Continue」**（繼續）。
4. 確認權限清單就是剛剛那四項，點 **「Authorize」**（授權）。
5. 完成「我不是機器人」驗證。
6. 看到「Authorized」或「已授權」就完成了。

### 3. 確認 bot 已經進來

1. 回到 Discord，打開你的伺服器。
2. 畫面**右邊的成員清單**（沒看到的話，點右上角的 **人像圖示** 打開）裡，應該有 `Food Logger`，旁邊有 **BOT**（或「應用程式」）標籤。
3. 現在它是**灰色離線**的，這是正常的。之後 agent 程式跑起來，它才會變成上線。

---

## 完成檢查

- [ ] Developer Portal 有 `Food Logger` 這個 application
- [ ] **MESSAGE CONTENT INTENT** 已打開，而且按過 Save Changes
- [ ] `.env` 裡有 `DISCORD_TOKEN=...`，`git status` 看不到 `.env`
- [ ] token 沒有貼在 `.env` 以外的任何地方
- [ ] 伺服器成員清單裡有這個 bot

做到這裡，`.env` 應該已經有 `DISCORD_TOKEN` 和 `CHANNEL_ID`。`CLAUDE_MODEL`、`DATA_DIR` 有預設值，可以先不填。
