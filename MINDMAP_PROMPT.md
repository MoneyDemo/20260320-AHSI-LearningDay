# ADO 工作項目互動式心智圖

## 情境
我是一位工程經理，負責管理 Azure DevOps (ADO) 的工作項目。這些工作項目有 4 層階層結構：Epic → Feature → User Story → Task。我的團隊會將工作項目匯出為 CSV 檔，CSV 包含 3 個區段（Basic Info、Relationships、Timeline），共 41 個欄位。其中關鍵欄位包括：Section、Work Item ID、Level、Type、Title、State、Priority、Story Points、Effort、Assigned To、Area Path、Sprint、Parent ID、Target Date、Comment Count、Related Link Count、Description Summary、Acceptance Criteria、Hierarchy Path、URL 等。

## 困難
ADO 的 Backlog 視圖只能顯示「縮排列表」— 一行一行的文字清單。它完全無法呈現一棵工作分解樹的「形狀」：哪條分支最深、哪條最淺、哪裡密集、哪裡稀疏、哪些節點沒有人負責。Power BI 也沒有原生的心智圖元件。

## 影響
缺少視覺化的樹狀全貌，我無法快速發現不平衡的分支（例如某個 Feature 底下堆了 20 個 Task，另一個卻是空的）、無法在會議中直覺地向利害關係人展示整個 Epic 的工作架構、也無法一眼看出哪些項目的狀態落後。

## 效益
一個互動式心智圖網頁應用，讓我上傳 CSV 後立即展開一棵可拖拉、可縮放、可互動的工作項目樹，每個節點用顏色和大小傳達關鍵資訊。這是用於 Demo 展示的工具，風格要明亮、專業、適合投影。

## 規格

### 資料載入
- 提供一個拖放區域（也可點擊選擇檔案）來上傳 CSV 檔。
- 拖放區樣式：虛線邊框的圓角矩形，中間顯示「📂 拖放 CSV 檔案至此，或點擊上傳」。
- CSV 中有一個 Section 欄位，篩選 Section === "Basic Info" 的列作為主要資料集。
- 載入後顯示摘要徽章：「✅ 已載入 X 個工作項目（Y Epic, Z Feature, W Story, V Task）」。
- 內建預載範例資料，讓應用程式不需上傳就能立即展示。使用以下範例：

```csv
Section,Work Item ID,Level,Type,Title,State,Priority,Story Points,Effort,Assigned To,Area Path,Sprint,Parent ID,Remaining Work,Completed Work,Original Estimate,Target Date,Comment Count,Related Link Count,Description Summary,Acceptance Criteria,Hierarchy Path,URL
Basic Info,207227,1,Epic,Golf Scorecard Tracker 2.1: Mobile App Integration for Pro Players,New,1,,6,Kristoff,eng\Tech\Security,Backlog,199801,,,,2026-04-25,8,3,API integration for mobile scorecard validation in tournaments.,4 sprints planned: Sprint 1 (Analysis) → Sprint 2 (Implementation) → Sprint 3 (Testing) → Sprint 4 (QA),Epic,https://dev.azure.com/ampx/eng/_workitems/edit/207227
Basic Info,207241,2,Feature,Add tests for GolfScorecardTool 2.1,Active,2,,,Elsa,eng\Tech\Security,Sprint 6,207227,,,,2026-03-28,5,2,Unit tests for API wrapper library.,"Increase test coverage, prevent regressions",Epic → Feature,https://dev.azure.com/ampx/eng/_workitems/edit/207241
Basic Info,209041,3,User Story,System Test,New,2,,,Elsa,eng\Tech\Security,Sprint 6,207241,,,,2026-03-28,3,3,Full tournament scorecard workflow system test.,Has 5 child tasks including #222776,Epic → Feature → User Story,https://dev.azure.com/ampx/eng/_workitems/edit/209041
Basic Info,222776,4,Task,Integrate GolfScorecardTool 2.1 --verify into golf-tournament-app scorecardvalidation,Active,2,2,,Elsa,eng\Tech\Security,Sprint 6,209041,14,2,16,2026-03-26,1,1,Integrate --verify command into scorecardvalidation suite.,"1) --verify integrated, 2) Test cases documented, 3) Tests pass",Epic → Feature → User Story → Task,https://dev.azure.com/ampx/eng/_workitems/edit/222776
```

### 心智圖佈局
- 根節點 = Level 最低的項目（Epic），位於畫面中央。
- 從 Parent ID 欄位建立父子關係，從根節點向外展開子節點。
- 子節點以放射狀分佈在父節點周圍，分支之間保持適當間距避免重疊。
- 整個心智圖可以用滑鼠滾輪縮放、拖拉背景平移。

### 節點外觀
- 節點形狀依 Type 區分：
  - Epic = 菱形
  - Feature = 六邊形
  - User Story = 圓形
  - Task = 圓角方形
- 節點填色依 State 區分：
  - New = #6B7280（灰色）
  - Active = #3B82F6（藍色）
  - Resolved = #F59E0B（琥珀色）
  - Closed = #10B981（綠色）
- 節點邊框依 Priority 區分：
  - P1 = 紅色粗邊框（3px）
  - P2 = 橘色邊框（2px）
  - P3 及其他 = 預設細邊框（1px）
- 節點大小依 Effort 或 Story Points 決定（取較大者；若兩者皆空則使用預設基礎大小）。較大的數值 = 較大的節點。
- 節點內顯示標籤：「#{Work Item ID}」第一行，「{Title}」第二行（超過 35 字元則截斷加省略號）。
- 文字顏色自動適應背景（深色背景用白字，淺色背景用黑字）。

### 連線外觀
- 父子節點間以曲線（貝茲曲線）連接。
- 連線顏色 = 父節點的 State 顏色，50% 透明度。
- 連線粗細 = 該分支的子孫總數越多越粗（最小 1px，最大 5px）。

### 互動行為
- **拖拉節點**：每個節點可以單獨拖拉到任意位置，其他節點會自動避讓。
- **懸停節點（Hover）**：顯示詳細資訊卡片，內容包含：
  - 完整標題
  - 類型 + 狀態（用顏色標籤顯示）
  - 負責人（Assigned To）
  - Sprint
  - 目標日期（Target Date）
  - 描述摘要（Description Summary）
  - 驗收條件（Acceptance Criteria）
  - 如果是 Task：顯示進度條（已完成 {Completed Work}h / 預估 {Original Estimate}h，剩餘 {Remaining Work}h）
  - 留言數（Comment Count）、關聯數（Related Link Count）
- **點擊節點**：在新分頁開啟該工作項目的 ADO URL。
- **雙擊節點**：收合或展開該節點的所有子節點，帶有平滑動畫過渡。

### 工具列（畫面頂部）
- 應用名稱：「ADO 工作項目心智圖」
- 上傳按鈕（觸發 CSV 上傳）
- 「全部展開」/「全部收合」按鈕
- 「置中」按鈕（將視圖重置回根節點為中心）
- 「匯出 PNG」按鈕（將當前心智圖截圖下載）

### 配色與風格
- 僅使用淺色模式（白色背景 #FFFFFF，面板表面 #F9FAFB，文字 #1F2937，邊框 #E5E7EB）。
- 適合投影展示：高對比度、大字體、明亮清爽。
- 不需要深色模式切換功能。

### 圖例（畫面右下角，半透明浮動面板）
- State 顏色圖例：灰=New、藍=Active、琥珀=Resolved、綠=Closed
- Type 形狀圖例：菱形=Epic、六邊形=Feature、圓形=User Story、圓角方形=Task
- Priority 邊框圖例：紅粗=P1、橘=P2、預設=P3
- 可點擊收合此圖例面板

### 統計面板（畫面左下角，半透明浮動面板）
- 各 State 的項目數量（帶小圓點顏色指示）
- 各 Type 的項目數量
- 負責人分佈（每人幾個項目）
- 可點擊收合此面板

### 響應式設計
- 滿版寬高，心智圖填滿可用空間。
- 視窗寬度 < 768px 時：收合左側篩選器和統計面板為圖示按鈕，圖例面板預設收合。

### 動畫
- 所有節點展開/收合使用 500ms ease 平滑過渡。
- 資料載入後，節點從中心向外逐層展開（先 Epic，再 Feature，再 Story，最後 Task），每層間隔 200ms，營造「生長」動畫效果。
- 篩選變更時，節點透明度以 300ms 過渡。
