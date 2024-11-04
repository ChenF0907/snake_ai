# AI 貪食蛇遊戲

## 專案簡介

這個專案是利用 PyTorch 製作的 AI 貪食蛇遊戲，並結合深度強化學習來訓練 AI 自動玩貪食蛇。目標是讓 AI 學會在遊戲中做出更好的決策，不斷提高分數。我們使用 Python 的 Pygame 庫來呈現遊戲畫面，並用深度 Q 網絡（DQN）來訓練 AI，讓它能夠學會如何避開障礙、吃掉食物。

## 目錄

- [專案簡介](#專案簡介)
- [安裝指南](#安裝指南)
- [使用說明](#使用說明)
- [遊戲演示](#遊戲演示)
- [系統需求](#系統需求)
- [功能特色](#功能特色)
- [模型架構](#模型架構)
- [強化學習算法](#強化學習算法)

## 安裝指南

### 1. 下載專案

首先，打開終端機，輸入以下指令來下載專案：

```bash
git clone https://github.com/Wei070901/snake_ai.git
cd snake_ai
```

### 2. 建立虛擬環境並安裝依賴

創建一個虛擬環境來管理專案的依賴：

```bash
python3 -m venv venv
source venv/bin/activate  # Windows 用戶執行 `venv\Scripts\activate`
pip install -r requirements.txt
```

### 3. 安裝 PyTorch

請前往 [PyTorch 官方網站](https://pytorch.org/)，根據您的系統選擇適合的安裝指令。

## 常見問題及故障排除

- **問題：PyTorch 未正確安裝**
  - **解決方案**：請確保使用與您的操作系統匹配的安裝指令，並檢查是否有足夠的磁碟空間和正確的網路連接。

- **問題：訓練過程中內存不足**
  - **解決方案**：嘗試降低 `batch_size`，減少每次訓練時使用的樣本數，或者在訓練時關閉其他占用大量內存的應用程序。

- **問題：訓練過程中收斂速度慢**
  - **解決方案**：調整 `learning_rate`，通常較大的學習率可以加速收斂，但也有可能導致不穩定，建議小幅度調整並觀察效果。

## 使用說明

### 1. 訓練 AI 模型

運行以下指令開始訓練 AI 模型：

```bash
python train.py
```

可以調整一些訓練參數，例如：

- `learning_rate`：學習率，控制模型每次更新時權重的變化幅度，較大的學習率可能加速收斂，但也容易導致不穩定，預設為 0.001。
- `epsilon`：Epsilon-Greedy 策略中的 epsilon 值，用於控制隨機行動的機率，預設為 1.0，隨訓練進行逐漸減少。
- `target_update_freq`：目標網絡的更新頻率，設定為每隔多少步數更新一次目標網絡，通常能有效提升模型的穩定性。
- `num_episodes`：訓練的回合數，預設為 1000。
- `batch_size`：每次訓練使用的樣本數，預設為 64。
- `gamma`：折扣因子，用來計算未來獎勵的現在價值，預設為 0.94。

### 2. 測試 AI 模型

訓練完成後，您可以使用以下指令來測試 AI 模型：

```bash
python train.py --test
```

在測試過程中，AI 會自動根據已學習的策略玩遊戲，並顯示最終得分。測試完成後，系統會輸出一些關鍵數據，例如得分總數和遊戲步數，這些信息可用於分析 AI 的表現。

### 3. 顯示遊戲畫面

在訓練過程中，您可以看到遊戲畫面，觀察 AI 的決策。設置 `render_mode=True` 可以開啟或關閉畫面顯示。

您還可以查看訓練過程中的詳細日志信息，這些信息會輸出到控制台，包含每一步的行動、獎勵和損失等數據。如果需要更全面地了解模型的訓練狀況，建議使用 TensorBoard 來查看訓練過程中的變化。

## 遊戲演示

![AI 貪食蛇遊戲演示](./game.gif)

上圖顯示了 AI 貪食蛇遊戲的實際操作畫面，紅色方塊為食物，藍色方塊為蛇。

## 系統需求

- Python 3.8 或更高版本
- PyTorch 1.9.0 或更高版本
- Pygame
- Numpy
- TensorBoard（可選，用於監控訓練過程）

## 功能特色

- **智能貪食蛇 AI**：使用深度 Q 網絡（DQN）來訓練 AI，讓它自主學習如何在遊戲中取得高分。
- **動態遊戲畫面**：使用 Pygame 來顯示遊戲畫面，方便實時觀察 AI 的行為。
- **經驗回放**：儲存遊戲中的經驗，讓 AI 能從過去的經驗中學習，提高訓練效果。
- **訓練監控**：使用 TensorBoard 監控訓練過程中的損失和獎勵變化。
- **雙重 DQN**：避免 Q 值的過度估計，提升模型的穩定性和性能。

## 模型架構

我們使用深度 Q 網絡（DQN）來進行強化學習，模型結構如下：

- **輸入層**：接收遊戲的狀態資訊，輸入大小為 12，代表蛇的位置、食物的位置等特徵。
  - **輸入大小說明**：輸入大小為 12 是由於我們將遊戲中的關鍵狀態信息（例如蛇的位置、食物的位置、方向等）進行了特徵提取，將這些信息轉換為 12 維的向量，以便神經網絡進行處理。
- **隱藏層**：包含兩個全連接層，每層有 256 個神經元，並使用 ReLU 激活函數來增加非線性。
  - **隱藏層細節**：這兩個隱藏層的設置是為了讓模型有足夠的能力去學習和表示遊戲中複雜的特徵。每層 256 個神經元可以讓模型有更多的參數來適應不同的遊戲場景。
  - **ReLU 激活函數**：可以讓模型學習到更複雜的特徵。
  - **Dropout 機制**：在隱藏層中加入 Dropout，防止過擬合，提升模型的泛化能力。
- **輸出層**：輸出每個可能行動的 Q 值，用於評估各種行動的預期回報。
  - **輸出層說明**：輸出層的大小等於可執行的動作數，例如貪食蛇可以選擇向上、向下、向左或向右。每個輸出代表相應動作的預期回報。
  - **Xavier 初始化**：使用 Xavier 方法來初始化權重，使得訓練更加穩定。

## 強化學習算法

我們採用了深度 Q 網絡（DQN），並結合以下技術來提升學習效果：

- **Epsilon-Greedy 策略**：在訓練過程中，AI 會以一定機率隨機選擇行動來探索，隨著訓練進行，這個機率會逐漸降低，讓 AI 更專注於已學到的策略。
- **經驗回放**：儲存每一步的狀態、行動、獎勵和下一狀態，隨機抽取樣本來訓練，打破資料的時間相關性，提升訓練穩定性。
- **雙重 DQN**：引入目標網絡來計算 Q 值，避免標準 DQN 在估計未來回報時的過度估計問題，提升模型的穩定性和性能。
- **折扣因子（Gamma）**：用來計算未來獎勵的折現值，讓模型既考慮短期收益，也能考慮長期策略的回報。
