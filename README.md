# 問心堂・公開多人連線版 架設說明

任何人打開網址都能進廟、互相看見、一起撞鐘擊鼓、共用光明燈牆，不需要 Claude 帳號。
需要兩樣免費服務：**Firebase**（多人連線資料）與 **GitHub Pages**（放網頁）。

## 一、建立 Firebase 專案（約 10 分鐘）

1. 到 https://console.firebase.google.com ，用 Google 帳號登入，按「建立專案」，名稱例如 `wenxintang`（Google Analytics 可關閉）。
2. 左側「建構」→ **Authentication** → 開始使用 → 「登入方式」分頁 → 選 **匿名** → 啟用 → 儲存。
3. 左側「建構」→ **Realtime Database** → 建立資料庫 → 位置選 **新加坡 (asia-southeast1)** → 選「**以鎖定模式啟動**」。
4. 進入 Realtime Database 的「**規則**」分頁，把 `database.rules.json` 的全部內容貼上，覆蓋原本內容 → **發布**。
5. 左上齒輪「專案設定」→「一般」→ 下方「你的應用程式」→ 按網頁圖示 `</>` → 取個名稱 → 註冊。
   畫面會出現一段 `firebaseConfig = { apiKey: ..., authDomain: ..., databaseURL: ..., projectId: ..., appId: ... }`。
   **請確認裡面有 `databaseURL`**（若沒有，代表第 3 步還沒完成，完成後回到專案設定重新複製）。

## 二、把設定放進網頁（擇一）

- **簡單做法**：把整段 `firebaseConfig` 貼給 Claude，Claude 會幫你填好 `index.html`。
  （這段設定本來就會出現在網頁原始碼中，不是密碼；安全靠第 4 步的規則把關。）
- **自己改**：用文字編輯器打開 `index.html`，搜尋 `const FIREBASE_CONFIG = null;`，把 `null` 換成那段 `{ ... }`。

## 三、放上網路（GitHub Pages）

本儲存庫已放好 `index.html`。開啟網站：
1. 儲存庫「Settings」→ 左側「Pages」。
2. Source 選「Deploy from a branch」，Branch 選「main」、資料夾「/ (root)」→「Save」。
3. 約 1–2 分鐘後網址為 `https://kevin9553.github.io/wenxintang/`。
4. 之後每次修改推上 `main`，網站會自動更新。

## 免費額度與注意事項

- Firebase 免費方案：**同時最多 100 人連線**、每月下載 10 GB。一般使用綽綽有餘；若常態有幾十人一起逛，可在 Firebase 升級為「Blaze 用量計費」方案。
- GitHub Pages 免費：每月約 100 GB 流量；網頁約 14 MB，約可支撐每月數千次造訪。
- 訪客第一次進入會被請求輸入暱稱（最多 10 字），其他人看得到；點右上角「在線」可以改名。
- 暱稱與光明燈心願是**公開**的。若有不當內容，到 Firebase「Realtime Database →資料」分頁，在 `pilgrims` 底下找到該筆刪除即可。
- 連不上 Firebase 時（設定錯誤或超過額度），網頁會自動改為單機模式，照常可以參拜。


## 檔案結構

- `index.html`：問心堂網頁本體（約 0.8 MB）。
- `models/*.glb`：每尊神像一個模型檔。網頁只在香客走近某座殿時才載入那座殿的神像，所以開啟快、手機也較順。
- 之後新增神明，只要在 `models/` 加一個檔案即可，不再受單一網頁大小限制。
- 注意：直接用檔案總管雙擊開啟 `index.html` 時，瀏覽器不允許讀取模型檔，神像會顯示為簡易造型；放上網站（GitHub Pages）就正常。
