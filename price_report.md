# GCP 服務計費方式整理報告

## 1. 報告目的
本報告用於整理 Google Cloud Platform（GCP）主要服務的計費方式，作為公司內部雲端成本預估、資源管理與帳單分析之依據。  
本文件以官方公開定價邏輯為基礎，並補充實務上常見的成本拆分方式。

## 2. 適用範圍
本報告涵蓋下列服務：

Compute Engine。
Cloud Run。
Kubernetes Engine。
Cloud SQL。
Memorystore。
Firestore。
Cloud Storage。
Filestore。
Cloud Key Management Service（Cloud KMS）。
Secret Manager。
Cloud Load Balancing。
Cloud Armor。
Vertex AI。
Vertex AI Search。


## 3. 計費原則總覽
GCP 多數服務採即付即用模式，但實際計費單位依產品而異。  
常見維度包含運算時間、儲存容量、請求次數、網路流量、預配資源與安全規則數量。  
內部報告建議將每個服務拆成「計費單位、主要成本來源、附加費用、是否預配型、是否有折扣」五個欄位。

## 4. 各服務計費方式

### 4.1 Compute Engine
Compute Engine 屬於典型預配型運算服務，主要依下列項目計費：

vCPU 使用量。
記憶體使用量。
磁碟容量與磁碟類型。
GPU 或其他加速器。
網路流量。


其特性是資源啟動後即持續計費，通常以時間為基礎累計。若有長期使用需求，還可能適用承諾折扣或使用折扣。

### 4.2 Cloud Run
Cloud Run 採用較彈性的請求導向計費模式，主要依下列項目收費：

CPU 使用時間。
記憶體使用時間。
請求數量。
執行時間。
網路流量。


其特色是只在實際執行期間計費，服務閒置時成本可大幅降低，適合流量波動明顯的應用。

### 4.3 Kubernetes Engine
Kubernetes Engine 的費用結構依部署模式不同而異：

標準模式通常以節點 VM 成本為主，另加叢集管理相關費用。
Autopilot 模式則依 Pod 資源使用量計費。


若使用額外的節點、附加儲存、負載平衡或網路資源，會另外產生成本。

### 4.4 Cloud SQL
Cloud SQL 的成本通常由以下部分組成：

資料庫實例費用。
CPU 與記憶體。
儲存空間。
備份。
網路流量。
高可用或複本資源。


Cloud SQL 屬於預配型資料庫服務，實例啟用後即持續產生成本。

### 4.5 Memorystore
Memorystore 主要針對 Redis 或 Memcached 等記憶體型快取服務，通常依以下方式計費：

預配記憶體容量。
節點規格。
地區。
版本或部署型態。


此服務屬於預配型資源，容量配置一旦建立便開始計費，不是按實際查詢量即時計費。

### 4.6 Firestore
Firestore 的計費通常較細，主要可能包含：

儲存量。
讀取次數。
寫入次數。
刪除次數。
網路傳輸。


Firestore 適合變動型應用，但若讀寫量高，請求費用可能快速累積，需特別注意使用模式。

### 4.7 Cloud Storage
Cloud Storage 的費用主要由下列部分構成：

儲存容量。
儲存類別。
物件操作次數。
資料取出費用。
跨區或對外網路傳輸。


不同儲存類別的單價不同，像是 Standard、Nearline、Coldline、Archive 的成本與存取頻率有明顯差異。

### 4.8 Filestore
Filestore 主要依下列項目計費：

配置容量。
服務層級。
地區。
效能設定。


若使用高階性能方案，可能另外涉及吞吐量或 IOPS 成本。此服務偏向預配型文件儲存，適合需要共享檔案系統的工作負載。

### 4.9 Cloud Key Management Service
Cloud KMS 的費用通常與以下項目相關：

Key versions 數量。
金鑰保護層級。
加解密操作次數。
HSM 或 EKM 類型。


此服務常被視為安全基礎設施成本的一部分，但在大量加解密場景中，操作次數也會形成明顯成本。

### 4.10 Secret Manager
Secret Manager 的主要成本來源包括：

Active secret versions。
Secret 存取次數。
旋轉通知或相關事件。


管理操作通常成本較低，但若服務架構頻繁讀取 secret，也需要納入使用量評估。

### 4.11 Cloud Load Balancing
Cloud Load Balancing 常見計費項目包括：

Forwarding rules。
流量處理費。
負載平衡器類型差異。
跨區或對外流量。


當系統對外服務流量增加時，負載平衡相關費用可能成為不可忽視的成本項目。

### 4.12 Cloud Armor
Cloud Armor 的成本通常依以下方式計算：

Policy 數量。
Rules 數量。
請求數量。
Enterprise 訂閱模式或受保護資源數量。


若網站或 API 流量大，且啟用較多安全規則，費用會隨之增加。

### 4.13 Vertex AI
Vertex AI 的計費較細，通常會依功能模組分開。  
官方定價頁提到的費用來源包含模型訓練、部署、預測、工作階段、記憶庫與相關 AI 服務用量。[2][1]

### 4.14 Vertex AI Search
Vertex AI Search 目前可見到兩種主要計費模式：一般即付即用模式，以及可設定價格模式。  
一般模式屬於用量型計費；可設定模式則提供訂閱門檻與加購機制，適合需要較明確預算控管的企業場景。[3][21]
此外，Vertex AI Search for commerce 的查詢費用也會依查詢類型而不同，官方範例顯示搜尋與瀏覽查詢可按每 1,000 次查詢計費。[21]

## 5. 成本觀察重點
預配型服務的核心風險是閒置資源持續計費。  
請求型服務的核心風險是高流量或高頻操作導致費用快速累積。  
儲存型服務要注意資料類別、操作次數與跨區流量。  
AI 類服務則要特別注意模型訓練、推論、token、查詢與資料儲存是否分開計費。[1][2][4][21]

## 6. 建議的內部欄位格式
建議每個服務至少保留以下欄位：

服務名稱。
產品類型。
計費模式。
計費單位。
主要成本項目。
是否預配型。
是否有免費額度。
是否適用折扣。
備註。


## 7. 結論
GCP 的費用並不是單一「月租」概念，而是由多種資源維度組成。  
若要用於公司內部管理，建議將每個服務拆解成可追蹤的成本項目，並另外註明 Vertex AI 與 Vertex AI Search 這類 AI 服務的獨立計費邏輯，以避免預算估算失真。[3][1]

若你要，我下一步可以直接幫你整理成 **正式的內部簡報版條列**，或 **可貼到 Word 的完整報告正文**。

來源
[1] Vertex AI 定價 https://cloud.google.com/vertex-ai/pricing?hl=zh-TW
[2] 在Vertex AI 中建構及部署AI 模型的費用 - Google Cloud https://cloud.google.com/vertex-ai/generative-ai/pricing?hl=zh-tw
[3] 為自訂搜尋設定可設定的價格| Vertex AI Search https://docs.cloud.google.com/generative-ai-app-builder/docs/enable-configurable-pricing?hl=zh-tw
[4] VM 執行個體定價 - Google Cloud https://cloud.google.com/products/compute/pricing?hl=zh-TW
[5] Cloud Run 定價 https://cloud.google.com/run/pricing?hl=zh-TW
[6] Google Kubernetes Engine 定價 https://cloud.google.com/kubernetes-engine/pricing?hl=zh-TW
[7] Cloud SQL 价格 https://cloud.google.com/sql/pricing?hl=zh-CN
[8] Memorystore for Redis pricing - Google Cloud https://cloud.google.com/memorystore/docs/redis/pricing
[9] Memorystore for Memcached pricing - Google Cloud https://cloud.google.com/memorystore/docs/memcached/pricing
[10] Firestore pricing https://cloud.google.com/firestore/pricing
[11] Pricing | Firestore - Firebase https://firebase.google.com/docs/firestore/enterprise/pricing
[12] The No BS Guide To GCP Storage Costs [Updated For 2026] https://www.cloudzero.com/blog/gcp-storage-pricing/
[13] Google Cloud Storage Classes & Pricing https://www.economize.cloud/blog/gcp-storage-classes-pricing-features-services/
[14] Filestore fully managed cloud file storage | Google Cloud https://cloud.google.com/filestore
[15] Cloud Key Management Service pricing https://cloud.google.com/kms/pricing
[16] Cloud Key Management | Google Cloud https://cloud.google.com/security/products/security-key-management
[17] Secret Manager pricing - Google Cloud https://cloud.google.com/secret-manager/pricing
[18] Cloud Load Balancing pricing https://cloud.google.com/load-balancing/pricing
[19] Cloud Load Balancing pricing https://cloud.google.com/load-balancing/pricing-announce
[20] Google Cloud Armor pricingcloud.google.com › armor › pricing https://cloud.google.com/armor/pricing
[21] Vertex AI Search 電子商務套件定價 - Google Cloud https://cloud.google.com/retail/pricing?hl=zh-tw
[22] 【Vertex AI 費用解析】有免費版嗎？一次看懂計費方式www.newspiggy.com › post › vertex-ai-pricing-analysis-free https://www.newspiggy.com/post/vertex-ai-pricing-analysis-free
[23] Vertex AI 是什麼？功能介紹、應用教學與費用一次看懂 https://www.newspiggy.com/post/vertex-ai-intro
[24] 定價 | Generative AI on Vertex AI | Google Cloud https://groundcanada.net/?hl=zh-tw&_=%2Fvertex-ai%2Fgenerative-ai%2Fpricing%23KJWqMdlUlBnoJ%2BIAVFPniITydIQuGFCs
[25] 定价| Vtrix API Docs https://doc.vtrix.ai/docs/zh-cn/getting-started/pricing
[26] AI系統價格：高效部署與成本優化的完整攻略 | AI人工智慧共和國 https://taiwan-ai.org/ai%E7%B3%BB%E7%B5%B1%E5%83%B9%E6%A0%BC
[27] 【 GCP 燒錢經驗分享：Vertex AI Search 】 󠀠 前幾天在測試skills ... https://www.facebook.com/groups/gdgcloudtaipei/posts/4333696790222492/
[28] Vertex AI 免費嗎？新手必看費用攻略 https://www.youtube.com/watch?v=ItkPvLG9bY8
[29] 預估每月儲存空間費用| Vertex AI Search https://docs.cloud.google.com/generative-ai-app-builder/docs/estimate-size-web-data?hl=zh-tw
[30] 【東東老師X 思想科技】Vertex AI 核心功能Model Garden 介紹 https://masterconcept.ai/zh-hant/learning-column/google-cloud-zh-hant/gcp-kol-x-master-concept-vertex-ai-core-function-model-garden-introduction/
[31] 運用Vertex AI 建構Google 等級的搜尋系統 https://codelabs.developers.google.com/build-google-quality-rag?hl=zh-tw
[32] Vertex AI 是什麼？功能介紹、應用教學與費用一次看懂 https://www.youtube.com/watch?v=e9v5T5KjsdI