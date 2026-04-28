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


---------------------------------------------------------------------------------

# Autopsy 鑑識結果樹狀檢視 (Tree View) 

本文件針對 Autopsy 鑑識軟體介面左側的樹狀檢視區域進行詳細說明，協助調查人員快速掌握案例（FOR_A888168）中的數位跡證分布。

---

## 1. 檔案視圖 (File Views)
此區塊從檔案系統的物理或邏輯屬性進行分類。

| 類別 | 說明 |
| :--- | :--- |
| **File Types** | 依據檔案副檔名或 MIME 類型（圖片、影片、執行檔等）進行自動歸類。 |
| **Deleted Files** | **重點區域。** 顯示被標記為刪除但尚未被覆蓋的檔案。鑑識官常在此尋找被銷毀的證據。 |
| **File Size** | 依據檔案大小區間進行篩選，有助於過濾大型資料庫或微小的系統檔。 |

## 2. 資料成品 / 數位跡證 (Data Artifacts)
此區塊顯示從系統與應用程式中提取出的具體行為紀錄，是重建使用者活動的核心。

| 項目 | 數量/狀態 | 鑑識意義 |
| :--- | :--- | :--- |
| **Communication Accounts** | 2 | 偵測到的通訊軟體帳號（如 Skype, Telegram）。 |
| **E-Mail Messages** | 1 | 提取出的電子郵件郵件內容。 |
| **Installed Programs** | 32 | 系統安裝過的程式清單，可用於判斷有無惡意工具或擦除軟體。 |
| **Recent Documents** | 8 | 使用者最近開啟過的文件或路徑。 |
| **Run Programs** | 81 | 程式執行紀錄（Prefetch/Registry），可證明特定軟體曾被運行。 |
| **Shell Bags** | 51 | 紀錄資料夾瀏覽歷史，即使資料夾已刪除亦可追蹤其路徑。 |
| **USB Device Attached** | 1 | 曾插入此電腦的外部儲存設備資訊（含序號與時間）。 |
| **Web Activity** | 887+ | 包含 Bookmarks(6)、Cookies(24)、History(887)、Search(4)。 |

## 3. 分析結果 (Analysis Results)
此區塊由 Ingest Modules 自動判斷出的異常或匹配項目。

| 項目 | 數量 | 調查重要性 |
| :--- | :--- | :--- |
| **Encryption Suspected** | 2 | 疑似加密檔案，可能包含受保護的敏感資訊。 |
| **Extension Mismatch Detected** | 9 | **高風險。** 發現 9 個檔案內容與副檔名不符（如隱藏的圖片或執行檔）。 |
| **Interesting Items** | 1 | 觸發預設規則的項目（如加密貨幣錢包或敏感關鍵字檔案）。 |
| **Keyword Hits** | 11,290 | 關鍵字搜尋匹配結果，需進一步過濾以縮小調查範圍。 |
| **Web Categories** | 4 | 自動對瀏覽過的網址進行內容分類（如社交、賭博等）。 |

## 4. 系統與管理項目
* **OS Accounts:** 列出系統內的使用者帳號（如 Administrator, Guest）。
* **Tags:** 顯示調查人員手動標記為「重要」或「待審查」的項目。
* **Score:** 根據自動化規則對各個項目的威脅或重要程度進行評分。
* **Reports:** 最終生成的鑑識報告彙整處。

---

## 鑑識調查建議
根據目前介面顯示的數據，建議優先調查以下項目：
1. **Extension Mismatch Detected (9 筆):** 調查這 9 個檔案為何要刻意隱藏原始格式。
2. **Encryption Suspected (2 筆):** 確認這兩個檔案是否為加密貨幣私鑰、密碼容器或機密文件。
3. **Web History (887 筆):** 分析使用者的上網習慣，特別是與本案相關的時間點。
