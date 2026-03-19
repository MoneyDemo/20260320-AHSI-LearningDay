## 討論過程與內容完整回顧

---

### 第 1 輪：初始提問與視覺化發想

**你的輸入**：提供了一份 4 筆工作項目的 CSV（原始版，20 個欄位），說明想用 GitHub Spark 針對 ADO 工作項目做一些 ADO 本身與 Power BI 做不到或困難才能做到的視覺化，初步想法包括心智圖或時序圖。

**我的產出**：推薦了 **15 個視覺化方案**，分為四大類：
- 階層/結構類（Mind Map、Sunburst、Treemap、Dendrogram）
- 時間/流程類（Gantt、Swimlane Timeline、Burndown、Calendar Heatmap）
- 關係/網路類（Force Graph、Sankey、Chord Diagram、DAG）
- 多維度/分析類（Radar、Bubble Chart、State Transition Matrix）

同時指出你的 CSV 缺少哪些欄位（如 Remaining Work、State Change History、Predecessor Links 等）。

---

### 第 2 輪：移除 ADO 已有的視覺化

**你的要求**：去除 ADO 本身就能做到的視覺化，例如 Kanban。

**我的產出**：明確列出被排除的 8 項（Kanban、Sprint Burndown、CFD、Velocity、Treemap、Bubble Chart、Calendar Heatmap、Lead/Cycle Time），保留了 **10 個方案** 並按現有欄位可行性排序（#1–#9 可直接做，#10–#12 需額外欄位）。

---

### 第 3 輪：加回 Treemap 和 Calendar Heatmap

**你的要求**：Treemap 和 Calendar Heatmap 要保留。

**我的產出**：更新清單為 **12 個視覺化**，重新整理成「現有欄位即可」9 個 + 「需額外欄位」3 個的結構。

---

### 第 4 輪：盤點需要額外匯出的欄位

**你的要求**：針對這 12 個視覺化，告訴你需要額外匯出哪些欄位。

**我的產出**：分為三層：
- **加分欄位**（讓 #1–#9 更好）：Remaining Work、Completed Work、Original Estimate、Closed Date、Activated Date、Description、Comment Count、Related Link Count
- **必要欄位**（#10–#12 必須有）：State Change History、Predecessor Links、Target Date、Resolved Date
- **優先級排序**：P0（CSV 勾選即可的 6 欄）→ P1（4 欄）→ P2（1 欄）→ P3（需 API）

---

### 第 5 輪：你提供了升級版 CSV

**你的輸入**：提供了全新的 CSV 結構 — 加入了 `Section` 欄位，將資料分為 3 個區段：
- **Basic Info**（4 列）：含 Description Summary、Acceptance Criteria、Hierarchy Path
- **Relationships**（8 列）：含 Relationship Type、Related ID/Type/Title（包括 Parent of、Ancestor of、Sibling of）
- **Timeline**（9 列）：含 Event Date、Event Type、Actor、State Change、Details

**你的問題**：「這樣的欄位夠了嗎？」

**我的產出**：逐一盤點 12 個視覺化的可行性變化 — 原本 3 個做不了的（#10 State Transition、#11 DAG、#12 Animated Timeline），現在 #10 和 #12 都可以做了（Timeline 的 State Change 事件解決了需求）。#11 仍缺 Predecessor 連結。仍建議補充 Remaining/Completed/Original Estimate 讓 Treemap/Mind Map 更好。

---

### 第 6 輪：我補完 CSV 欄位

**你的要求**：直接幫你編輯 CSV，補上缺少的 9 個欄位並生成合理的模擬資料。

**我的操作**：
1. 用 PowerShell 腳本新增 9 欄：Remaining Work、Completed Work、Original Estimate、Activated Date、Resolved Date、Closed Date、Target Date、Comment Count、Related Link Count
2. 過程中發現 Timeline 行有 off-by-one 欄位偏移問題，修復了 3 次
3. 修正 Basic Info 的 URL 錯位（URL 卡在 Details 欄）
4. 修正 222776 的 Timeline URL 缺失

**生成的合理數據邏輯**：
- Task 222776：2 SP ≈ 16h，剛建立做了 2h，剩 14h
- Activated Date：從 Timeline 的 State Change 事件推導
- Target Date：Epic 4 Sprint 規劃 → 04-25；Feature/Story Sprint 6 內 → 03-28；Task 一週 → 03-26
- Comment Count：Epic(8) > Feature(5) > Story(3) > Task(1)

---

### 第 7 輪：完整 Review CSV

**你的要求**：重新完整 Review CSV 的正確性與合理性。

**我的操作**：用 PowerShell 跑了 8 項檢查，全部 PASS：
1. 結構：21 行 × 41 欄
2. 一致性：同 ID 跨 Section 的值一致
3. 階層：L1→L2→L3→L4 逐層遞增，Parent ID 對應正確
4. 日期時序：Created ≤ Activated ≤ LastUpdated ≤ Target
5. Timeline 時序：EventDate ≥ CreatedDate
6. 新欄位邏輯：RW+CW=OE、Active 有 ActivatedDate、New 無 ActivatedDate
7. URL：全部 21 行都有正確 URL
8. Section 隔離：無交叉污染

---

### 第 8 輪：重新整理視覺化清單

**你的要求**：根據最終版 CSV 重新整理可做的視覺化。

**我的產出**：最終 **11 個可做的視覺化**（#11 DAG 仍缺 Predecessor），分為四類並附上每個使用的具體欄位。推薦實作順序：
1. Interactive Mind Map
2. Animated Timeline
3. Force-Directed Graph
4. Treemap
5. Calendar Heatmap

---

### 第 9 輪：Animated Timeline 的 SCIB Prompt

**你的要求**：先做 Animated Timeline，給一個 GitHub Spark 的 SCIB 框架 Prompt。

**我的產出**：完整的 Animated Timeline prompt，包含：
- SCIB 四段結構
- 詳細的 Specification（資料載入、泳道設計、事件標記、播放控制、事件日誌面板、顏色方案、響應式設計）
- 內嵌完整的範例 CSV 資料
- 技術棧：D3.js + PapaParse + html2canvas

---

### 第 10 輪：五個視覺化的效益分析表

**你的要求**：給一個表格說明每個視覺化帶來哪些益處。

**我的產出**：按三個角色（工程師 / EM-Tech Lead / PM-利害關係人）分析每個視覺化的具體效益，加上「ADO/PBI 完全做不到的獨特價值」欄。最後用一個流程圖展示五圖的互補關係：結構 → 時間 → 關係 → 資源 → 節奏。

---

### 第 11 輪：心智圖 SCIB Prompt（不指定技術框架）

**你的要求**：給心智圖的 Prompt，繁體中文，用於 GitHub Spark 所以不指定技術框架。

**我的產出**：完整的 SCIB Prompt，包含節點形狀/顏色/大小的映射規則、互動行為、工具列、圖例、統計面板、篩選器、動畫等。

---

### 第 12 輪：移除深色模式

**你的要求**：這是 Demo 用的，不要深色模式。

**我的產出**：移除深色模式切換功能，固定為淺色明亮風格（白色背景、高對比度），適合投影展示。

---

### 第 13 輪：替換人名去識別化

**你的操作**（在 Terminal 中）：將 CSV 中的人名替換為通用名（Anna→Alice、Elsa→Emily、Hans→Henry、Kristoff→Kevin），URL 也從 `ampx/eng` 改為 `demo/proj`。

---

### 關鍵決策節點摘要

| 輪次 | 決策 | 影響 |
|:----:|------|------|
| 2 | 剔除 ADO 已有的視覺化 | 從 15 個砍到 10 個，聚焦差異化價值 |
| 3 | 加回 Treemap + Calendar Heatmap | 最終 12 個 |
| 5 | 你提供了 3-Section CSV 結構 | 解鎖了 State Transition 和 Animated Timeline |
| 6 | 補完 9 個欄位 | 11 個視覺化可做，只剩 DAG 缺資料 |
| 9 | 決定先做 Animated Timeline | 從原本的 Mind Map 優先改為 Timeline 優先 |
| 11 | 改回先做 Mind Map | 最終第一個 Prompt 是心智圖 |
| 12 | 移除深色模式 | Demo 導向的設計決策 |