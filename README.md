# 🎄 聖誕交換禮物抽獎系統

一個即時同步的聖誕節禮物交換抽獎網頁，支援手機掃碼加入、電視大螢幕顯示。

## 📁 檔案結構

```
gift-exchange/
├── index.html    # 電視端主畫面（1920x1080）
├── join.html     # 手機端填寫頁面
└── README.md     # 設定說明
```

## 🔥 Firebase 設定步驟

### 步驟 1：建立 Firebase 專案

1. 前往 [Firebase Console](https://console.firebase.google.com/)
2. 點擊「新增專案」
3. 輸入專案名稱（例如：`christmas-gift-exchange`）
4. 可以關閉 Google Analytics（這個專案不需要）
5. 點擊「建立專案」

### 步驟 2：啟用 Realtime Database

1. 在左側選單點擊「Build」→「Realtime Database」
2. 點擊「建立資料庫」
3. 選擇資料庫位置（建議選擇 `asia-southeast1` 新加坡）
4. **重要**：選擇「以測試模式啟動」（這樣才能讀寫資料）
5. 點擊「啟用」

### 步驟 3：啟用 Storage（存放照片）

1. 在左側選單點擊「Build」→「Storage」
2. 點擊「開始使用」
3. **重要**：選擇「以測試模式啟動」
4. 選擇位置（與 Database 相同即可）
5. 點擊「完成」

### 步驟 4：取得設定資訊

1. 點擊左上角的齒輪圖示 ⚙️ →「專案設定」
2. 往下捲動到「您的應用程式」區塊
3. 點擊網頁圖示 `</>`
4. 輸入應用程式暱稱（例如：`gift-exchange-web`）
5. **不需要**勾選「Firebase Hosting」
6. 點擊「註冊應用程式」
7. 你會看到類似以下的設定碼：

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyB...",
  authDomain: "your-project.firebaseapp.com",
  databaseURL: "https://your-project-default-rtdb.firebaseio.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

### 步驟 5：更新程式碼

1. 複製上面的 `firebaseConfig` 設定
2. 開啟 `index.html`，找到這段程式碼並替換：

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    ...
};
```

3. 開啟 `join.html`，同樣找到並替換 `firebaseConfig`

### 步驟 6：設定安全規則（重要！）

#### Realtime Database 規則

1. 在 Firebase Console，進入「Realtime Database」
2. 點擊「規則」標籤
3. 將規則改為：

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

4. 點擊「發布」

⚠️ **注意**：這個規則是完全開放的，僅適合短期活動使用。活動結束後建議刪除專案。

#### Storage 規則

1. 在 Firebase Console，進入「Storage」
2. 點擊「Rules」標籤
3. 將規則改為：

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if true;
    }
  }
}
```

4. 點擊「Publish」

## 🚀 部署到 GitHub Pages

### 步驟 1：建立 GitHub Repository

1. 前往 [GitHub](https://github.com)
2. 點擊右上角 `+` → `New repository`
3. Repository 名稱：`christmas-gift-exchange`
4. 設為 Public
5. 點擊「Create repository」

### 步驟 2：上傳檔案

1. 點擊「uploading an existing file」
2. 將 `index.html` 和 `join.html` 拖曳上傳
3. 點擊「Commit changes」

### 步驟 3：啟用 GitHub Pages

1. 進入 Repository 的「Settings」
2. 左側選單點擊「Pages」
3. Source 選擇「Deploy from a branch」
4. Branch 選擇「main」，資料夾選「/ (root)」
5. 點擊「Save」
6. 等待 1-2 分鐘，網址會顯示在上方

你的網站網址會是：
```
https://你的帳號.github.io/christmas-gift-exchange/
```

## 📱 使用流程

### 活動前準備

1. 電腦連接到 65 吋電視（HDMI 或無線投影）
2. 開啟瀏覽器，進入 `index.html` 頁面
3. 按 F11 進入全螢幕模式
4. 確認右上角顯示「✅ Firebase 已連線」

### 入場報到

1. 參加者用手機掃描左下角 QR Code
2. 填寫姓名並上傳/拍攝照片
3. 點擊「加入抽獎」
4. 成功後會在電視畫面的球池中看到自己

### 開始抽獎

1. 主持人在右上角控制台選擇第一位抽獎者
2. 點擊「確認指定」
3. 被指定的人點擊紅色「開始抽獎」按鈕
4. 等待 5 秒動畫後顯示中獎者
5. 中獎者繼續點擊按鈕抽下一位
6. 重複直到所有人都被抽完

## ❓ 常見問題

### Q: Firebase 顯示連線失敗？
A: 檢查 `firebaseConfig` 設定是否正確，特別是 `databaseURL` 要包含完整網址。

### Q: 照片上傳失敗？
A: 確認 Firebase Storage 已啟用，且規則設為允許讀寫。

### Q: QR Code 掃描後打不開？
A: 確認 GitHub Pages 已部署完成，網址可以正常訪問。

### Q: 畫面在電視上顯示不完整？
A: 確認瀏覽器縮放比例為 100%，並按 F11 進入全螢幕。

## 🎨 自訂選項

### 修改動畫時間
在 `index.html` 中找到：
```javascript
const duration = 5000; // 5秒，可改為其他數值
```

### 修改標題文字
在 `index.html` 的 `<header>` 區塊修改。

### 修改配色
在 CSS 的 `:root` 區塊修改顏色變數。

---

🎅 祝活動順利，聖誕快樂！🎄
