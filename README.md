# 1 對 1 免費諮詢預約頁

灰哥的免費諮詢預約頁。單一 HTML 檔，沒有後端、沒有資料庫，樣式和程式都寫在 `index.html` 裡面。

- **本機檔案**：`~/Documents/consult/index.html`
- **線上網址**：（尚未上線，見下方「上線步驟」）
- **在漏斗的位置**：Threads 導流 → **這一頁**（免費諮詢）→ 諮詢中提案 → [三個月理財陪跑](https://geniusjohn0502.github.io/plan-2028/3month/)（NT$8,000）

---

## 表單怎麼運作（2026-10-08 改版）

頁面上的題目是**自己畫的**，不是嵌入 Google 表單的框框。訪客按送出時，答案會直接寄到 Google 表單的收件口，**照樣出現在 Google 表單後台的「回覆」分頁和連動的試算表**。

- 對應的 Google 表單：「財務健檢預約表」（`1FAIpQLSfbHXLWdDFaYh73T7X0OrvRRMziJPBCb-BTKyGis6uUYVygCw`）
- 每一題的 `name="entry.數字"` 就是 Google 表單那題的編號
- Email 欄位的 `name="emailAddress"` 對應 Google 表單「設定 → 回覆 → 收集電子郵件地址：**回覆者自行輸入**」。⚠️ **不能改回「已驗證」**——那個設定要求填表人先登入 Google，從這頁送出的答案會全部被拒收（2026-10-08 實測過）
- 送出後畫面換成「收到了」那一塊，帶 Calendly 預約按鈕和加 LINE 提醒

> ⚠️ **在 Google 表單裡改了題目，這頁要跟著改。**
> 新增必填題、改選項文字、改題型，都會讓這頁送出的答案對不上，Google 會**默默拒收**——訪客看到「收到了」，你後台卻什麼都沒有。
> 改完 Google 表單後，請金特務重新抓一次題目編號、更新 `index.html` 的表單區塊，再實際送一筆測試。
>
> 只改題目的**說明文字**（不改選項）不受影響。

Calendly 網址在 `index.html` 最底下的 `CALENDLY_URL`。

---

## 上線步驟

### 一、放上 GitHub Pages

```bash
cd ~/Documents/consult
git init && git add . && git commit -m "新增免費諮詢預約頁"
gh repo create consult --public --source=. --push
```

然後到 GitHub 的 repo 頁面：**Settings → Pages → Source** 選 `main` 分支、資料夾選 `/ (root)`，按 Save。

等一兩分鐘，頁面就會出現在 `https://geniusjohn0502.github.io/consult/`。

> **為什麼要開新的 repo，不放進 plan-2028？**
> 一個 GitHub repo 只能綁定一個自訂網域。如果讓 plan-2028 綁上新網域，三年百萬和三個月陪跑那幾頁的網址會跟著一起變。開新的，兩邊互不影響。

### 二、綁上你自己的網域

**先在 repo 根目錄建一個叫 `CNAME` 的檔案**（沒有副檔名），裡面只寫一行你要用的子網域，例如：

```
consult.你的網域.com
```

**再到 Cloudflare 設定 DNS**（這一步要你自己操作）：

1. 進 Cloudflare → 選你的網域 → 左邊選 **DNS**
2. 按 **Add record**，填：
   - Type：`CNAME`
   - Name：`consult`（就是子網域那一段，不用打全名）
   - Target：`geniusjohn0502.github.io`
   - **Proxy status：點一下那朵雲，把它從橘色切成灰色（DNS only）**
3. 左邊選 **SSL/TLS → Overview**，模式選 **Full**

> ⚠️ **這兩個設定最容易踩雷，講清楚會發生什麼事：**
>
> - **雲朵沒切成灰色**：GitHub 沒辦法驗證這個網域是你的，安全憑證會一直發不出來，頁面卡在「憑證處理中」進不去。
> - **SSL 模式選成 Flexible**：網頁會一直重新導向自己，訪客只會看到「重新導向次數過多」的錯誤，整頁打不開。

### 三、回 GitHub 完成

Settings → Pages → **Custom domain** 填入 `consult.你的網域.com`，按 Save。

等 10–30 分鐘讓憑證發出來，然後回到同一頁勾選 **Enforce HTTPS**。勾得起來就代表成功了。

---

## 上線後要測的事

- [ ] 網址打得開，網址列有鎖頭圖示
- [ ] 手機打開不會左右跑版
- [ ] **實際自己送出一次表單**，確認畫面會換成「收到了」那一塊，Calendly 按鈕點得開
- [ ] 測試完記得去 Google 試算表把自己那筆測試資料刪掉

---

## 之後想改內容

整頁的文字都在 `index.html` 裡面，直接搜尋你要改的那句話就找得到。改完存檔，然後：

```bash
cd ~/Documents/consult && git add . && git commit -m "更新文案" && git push
```

推上去大約一分鐘後線上就會更新。

---

## 這頁的內容來自哪裡

寫的時候不是憑空想的，素材出處記在這裡，日後要改才知道回哪裡查：

| 頁面區塊 | 來源 |
|:--|:--|
| 痛點五條 | `200_Reference/business/26份用戶訪談痛點分析.md`、`ideal-customer.md` 的〈客戶原話痛點〉 |
| 「先跑再算」三步 | `100_Todo/projects/2026-08-03_三個月陪跑銷售頁.md`（2026-08-03 命名定案） |
| 45 分鐘三個問題 | `品牌訪談_2026-08-21_整理版.md`「開場的三個問題」 |
| 學員見證 | `~/Documents/灰哥財務諮詢/個案見證/` 10 張截圖辨識成文字 |
| 關於灰哥 | `about-me.md` ＋ 品牌訪談「30 歲才開始理財」 |

> ⚠️ **對外數據口徑**（出自 `about-me.md`，不要自行更動）：投資經歷寫 **10 年**（不是 12，這是刻意保守的選擇）、陪跑學員約 50 位、不對外提房地產持有數。

> ⚠️ **這頁刻意不談槓桿、信貸、股票質押**。那是給已經有投資紀律的舊學員的題材，2026/07/19 已經驗證過：拿到前端通路會引來投資客，互動率掉到 0.4%。
