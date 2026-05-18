# MedStats — 醫療檢驗執行次數統計平台

## 專案簡介
MedStats 是一套醫療檢驗執行次數統計平台 Demo，提供各合作醫院每月執行次數查詢、統計圖表、報修紀錄管理等功能。  
本 Demo 版本所有資料均為內建展示資料，**無需後端 API 或資料庫**，可直接於靜態頁面（GitHub Pages / IIS）運行。

> ⚠️ **Demo 版本說明**：此版本為展示用途，帳號驗證及資料新增/修改僅作用於當次瀏覽器 Session 記憶體，重新整理後將還原為預設資料。

---

## 功能列表

| 功能 | 管理員 | 系統員工 | 合作單位 |
|------|:------:|:-------:|:--------:|
| 總覽儀表板 | ✓ | ✓ | ✓ |
| 執行次數查詢 | ✓ | ✓ | ✓ |
| 下載統計報表 | ✓ | ✓ | ✓ |
| 輸入執行次數 | ✓ | ✓ | — |
| 報修紀錄（新增/查詢） | ✓ | ✓ | — |
| 帳號管理 | ✓ | — | — |
| 醫院管理 | ✓ | — | — |

---

## Demo 測試帳號

| 帳號 | 密碼 | 角色 |
|------|------|------|
| admin | 1234 | 最高權限管理員 |
| staff | 1234 | 系統員工 |
| partner | 1234 | 合作單位人員 |

---

## GitHub Pages 部署（靜態展示）

### 步驟

```bash
# 1. 建立 GitHub Repository
# 前往 https://github.com/new 建立新 repo（建議設為 Public）

# 2. 上傳檔案
git init
git add demo-dashboard.html
git add Taiwan_map.jpg
git add README.md
git commit -m "Initial commit: MedStats demo dashboard"
git branch -M main
git remote add origin https://github.com/YOUR_ORG/medstats-demo.git
git push -u origin main

# 3. 啟用 GitHub Pages
# Settings → Pages → Source: Deploy from branch → main → / (root) → Save
```

### 訪問網址
```
https://YOUR_ORG.github.io/medstats-demo/demo-dashboard.html
```

> **注意**：地圖圖片 `Taiwan_map.jpg` 需與 HTML 檔案放在同一目錄下。

---

## IIS 部署（落地展示環境）

### 系統需求
- Windows Server 2016 / 2019 / 2022
- IIS 10.0 以上
- 磁碟空間：至少 100MB

### 安裝步驟

```powershell
# 步驟 1：安裝 IIS
Install-WindowsFeature -Name Web-Server -IncludeManagementTools

# 步驟 2：建立網站資料夾
New-Item -ItemType Directory -Path "C:\inetpub\medstats"

# 步驟 3：複製檔案
Copy-Item "demo-dashboard.html" -Destination "C:\inetpub\medstats\index.html"
Copy-Item "Taiwan_map.jpg"      -Destination "C:\inetpub\medstats\Taiwan_map.jpg"

# 步驟 4：建立應用程式集區與網站
New-WebAppPool -Name "MedStatsPool"
Set-ItemProperty IIS:\AppPools\MedStatsPool -Name "managedRuntimeVersion" -Value ""
New-Website -Name "MedStats" `
  -Port 8080 `
  -PhysicalPath "C:\inetpub\medstats" `
  -ApplicationPool "MedStatsPool"

# 步驟 5：開放防火牆
New-NetFirewallRule -DisplayName "MedStats HTTP" `
  -Direction Inbound -Protocol TCP -LocalPort 8080 -Action Allow

# 步驟 6：啟動網站
Start-Website -Name "MedStats"
```

瀏覽器開啟：`http://SERVER_IP:8080`

---

## 技術架構（Demo 版本）

- **前端**：純 HTML / CSS / JavaScript（無框架依賴）
- **圖表**：Chart.js 4.4.1（CDN）
- **字型**：Noto Sans TC（Google Fonts）
- **資料**：全部內建於 JavaScript，無需任何後端或資料庫
- **地圖**：靜態圖片 `Taiwan_map.jpg` 搭配 CSS 定位標記

---

## 後續正式版本規劃

### Phase 2：後端整合
- 資料庫：SQL Server / PostgreSQL
- 後端：ASP.NET Core / Node.js
- 使用者驗證：JWT Token + 資料庫驗證

### Phase 3：進階功能
- 電子郵件通知（每月報表自動寄送）
- 手機版 APP
- 進階預測模型（ARIMA / Prophet）
- HIS 系統 API 整合

---

## 檔案清單

```
medstats-demo/
├── demo-dashboard.html   ← 主程式（單一 HTML 包含所有 CSS / JS）
├── Taiwan_map.jpg        ← 台灣地圖圖片
└── README.md
```
