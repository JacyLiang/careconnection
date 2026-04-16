# CareConnection 手勢辨識與長照輔助系統
CareConnection 是一個基於 **MediaPipe** 手勢辨識技術的專案，旨在透過分析手部關節節點座標與向量變化，辨識特定的手勢動作。此系統整合了數據採集、特徵工程、多種機器學習/深度學習模型訓練，並提供網頁端介面與 LineBot 整合應用。
---
## 功能特點
即時數據採集：使用 OpenCV 與 MediaPipe Hands 獲取手部 21 個關節點的 3D 座標。

特徵工程：計算關節間的「相對向量」與「絕對座標」，提高模型對不同手掌大小與位置的魯棒性。

多模型支援：包含傳統機器學習 (SVM, Random Forest) 與深度學習 (DNN, 2D-CNN) 辨識方案。

### 全方位應用：

Web 端：提供基於 Flask 的管理後台與 MediaPipe JS 的前端辨識頁面。

LineBot：整合通訊軟體，實現遠端通報或互動功能。
---
## 目錄結構
Plaintext
└── jacyliang-careconnection/
    ├── DataCollection/     # 原始數據採集 (透過鏡頭錄製並生成 MP4/圖片)
    ├── DataProcessing/     # 預處理腳本 (將影片轉為向量與座標 CSV，設定 4fps 採樣)
    ├── Model/              # 模型訓練 (包含 CNN, DNN, SVM 等多種實驗模型)
    ├── Web/                # Flask 網頁應用程式 (管理介面與即時辨識前端)
    └── LineBot/            # Line 機器人整合 (app.py)
---
## 環境需求
本專案建議使用 Python 3.8+，主要依賴項如下：

核心處理：opencv-python, mediapipe

數據與模型：numpy, pandas, tensorflow, scikit-learn

後端框架：flask

其他：csv, inspect, time

注意：tensorflow 2.9.0 與 mediapipe 在某些環境下可能存在 flatbuffers 版本衝突，建議使用虛擬環境處理。

---
## 數據處理流程
影片採集：執行 DataCollection/test.ipynb 錄製手勢影片。

特徵轉化：執行 DataProcessing/向量+座標_to_csv_4fps.ipynb。

將 21 個手部節點轉化為相對向量（例如：thumb_Tip - thumb_Ip）。

輸出的 CSV 包含 124 個特徵欄位（60 個向量特徵 + 63 個原始座標 + 1 個類別標籤）。

自動根據檔名首位數字進行分類標記。
---
## 模型辨識類別 (Class Mapping)
根據程式碼邏輯，手勢被歸類為以下 5 類：

Class 0: 原手勢 1

Class 1: 原手勢 2

Class 2: 原手勢 3, 4

Class 3: 原手勢 5, 7

Class 4: 原手勢 6, 8
