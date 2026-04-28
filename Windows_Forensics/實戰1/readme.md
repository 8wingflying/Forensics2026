## Autopsy技術分析階段：
- 第一階段：建立分析案件相關資料
- 第二階段：將相關檔案匯入
- 第三階段：進行各種鑑識分析

## 

# Autopsy Ingest Modules (攝入模組) 功能說明清單

本文件整理了 Autopsy 數位鑑識軟體中各個攝入模組（Ingest Modules）的功能說明，協助調查人員理解在分析案例資料時，各模組所扮演的角色。

---

## 1. 基礎檔案分析與識別 (Basic File Analysis)

| 模組名稱 | 功能說明 |
| :--- | :--- |
| **File Type Identification** | 根據檔案的「標頭資料（Magic Number）」而非副檔名來識別檔案的真實類型。 |
| **Extension Mismatch Detector** | 比對檔案內容類型與其副檔名是否一致，用以發現企圖透過更改副檔名來隱藏資料的行為。 |
| **Hash Lookup** | 計算檔案雜湊值（MD5/SHA），並比對已知資料庫（如 NSRL），用以過濾已知系統檔案或識別已知惡意程式。 |
| **Data Source Integrity** | 驗證原始映像檔的雜湊值，確保鑑識過程中資料未被竄改，維持證據完整性。 |

## 2. 資料提取與復原 (Data Extraction & Recovery)

| 模組名稱 | 功能說明 |
| :--- | :--- |
| **Embedded File Extractor** | 自動解壓縮（ZIP, RAR 等）或提取文件（docx, pdf）中嵌入的資源檔案。 |
| **PhotoRec Carver** | 從磁碟的「未分配空間」中，根據檔案特徵碼（Carving）嘗試復原已刪除的檔案。 |
| **Virtual Machine Extractor** | 偵測並分析映像檔內的虛擬機磁碟格式（如 VMDK, VHD）。 |

## 3. 使用者活動與內容分析 (User Activity & Content)

| 模組名稱 | 功能說明 |
| :--- | :--- |
| **Recent Activity** | 提取作業系統關鍵資料，如網頁瀏覽紀錄、下載清單、最近開啟文件（Registry/Prefetch）等。 |
| **Keyword Search** | 對檔案內容進行索引，允許調查人員使用關鍵字或正規表示式（Regex）進行搜尋。 |
| **Email Parser** | 解析電子郵件格式（MBOX, PST, EML），提取郵件本文與附件資料。 |
| **Picture Analyzer** | 提取圖片的 EXIF 中繼資料（如座標、器材）並產生縮圖以供快速閱覽。 |
| **GPX Parser** | 專門解析 GPS 軌跡檔案，提取地理位置與路徑資訊。 |

## 4. 進階安全性與特殊設備分析 (Security & Advanced Analysis)

| 模組名稱 | 功能說明 |
| :--- | :--- |
| **Encryption Detection** | 偵測檔案或磁碟分割區是否經過加密（如 BitLocker, VeraCrypt）。 |
| **YARA Analyzer** | 使用自定義的 YARA 規則掃描檔案，尋找特定的惡意軟體特徵。 |
| **Cyber Triage Malware Scanner** | 整合外部雲端服務分析檔案是否具備惡意特徵（圖中預設未勾選）。 |
| **DJI Drone Analyzer** | 專門解析大疆（DJI）無人機產生的紀錄檔與媒體資料。 |

## 5. 行動裝置分析 (Mobile Forensics)

| 模組名稱 | 功能說明 |
| :--- | :--- |
| **Android Analyzer (aLEAPP)** | 使用 aLEAPP 工具解析 Android 裝置中的通訊錄、簡訊、通話紀錄與 App 資料。 |
| **iOS Analyzer (iLEAPP)** | 使用 iLEAPP 工具針對 iOS 裝置（iPhone/iPad）的備份或映像檔進行解析。 |
| **Android Analyzer** | 基礎的 Android 資料來源分析模組。 |

## 6. 關聯分析與管理 (Management & Correlation)

| 模組名稱 | 功能說明 |
| :--- | :--- |
| **Interesting Files Identifier** | 根據預設規則（路徑、檔名）自動標記調查人員可能感興趣的敏感檔案。 |
| **Central Repository** | 將分析結果與過去案例比對，尋找跨案例的關聯性（如相同的雜湊值）。 |
| **Plaso** | 強大的時間軸工具，從各種紀錄檔提取時間戳記，生成超大規模時間軸（Super Timeline）。 |

---
## 使用建議
* **初次分析：** 優先勾選 `File Type Identification`、`Hash Lookup` 與 `Recent Activity`。
* **深挖資料：** 若需要尋找被刪除的檔案，再執行 `PhotoRec Carver`（耗時較長）。
* **行動裝置：** 針對行動裝置映像檔，務必勾選 `aLEAPP` 或 `iLEAPP` 以獲得最佳解析效果。
