# GCP 服務計費方式整理

一個以 HTML 靜態網站呈現的 Google Cloud Platform 主要服務計費邏輯整理，適合作為公司內部雲端成本預估、資源管理與帳單分析的參考文件。

## 🌐 線上瀏覽

> 部署後將網址填入此處，例如：`https://你的帳號.github.io/你的repo名稱/`

## 📋 涵蓋服務

| 類別 | 服務 |
|------|------|
| 運算 | Compute Engine、Cloud Run、Kubernetes Engine |
| 儲存 | Cloud SQL、Memorystore、Firestore、Cloud Storage、Filestore |
| 安全 | Cloud KMS、Secret Manager |
| 網路 | Cloud Load Balancing、Cloud Armor |
| AI | Vertex AI、Vertex AI Search |

## 🗂 功能頁面

- **服務總覽** — 14 項服務卡片一覽，點擊可跳轉對應分類
- **運算 / 儲存 / 安全 / 網路 / AI** — 各服務計費項目與注意事項
- **彙整表格** — 所有服務的計費模式、預配型、折扣一覽表
- **成本觀察** — 預配型、請求型、儲存型、AI 服務的風險與建議

## 🚀 使用方式

直接用瀏覽器開啟 `index.html` 即可，無需任何伺服器或安裝。

```
index.html   ← 開啟這個檔案
```

## 📌 GitHub Pages 部署

1. 將此 repository 推上 GitHub
2. 進入 repo 的 `Settings → Pages`
3. Source 選擇 `main` branch，點擊 Save
4. 稍等片刻，即可透過公開網址瀏覽

## 📄 資料來源

內容以 [Google Cloud 官方定價頁](https://cloud.google.com/pricing) 公開定價邏輯為基礎整理，僅供內部參考使用。
