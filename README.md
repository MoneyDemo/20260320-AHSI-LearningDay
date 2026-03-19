# AHSI Learning Day — GitHub Spark Workshop

> **日期：** 2026 年 3 月 20 日  
> **主題：** 使用 GitHub Spark 進行互動式視覺化與網站生成

---

## 簡介

本專案是 AHSI Learning Day 的 Workshop 教材與素材集合，目標是讓學員透過 **GitHub Spark** 學會用自然語言 Prompt 快速建立互動式網頁應用。

Workshop 涵蓋兩大實作主題：

1. **互動旅行行程網站** — 以新加坡 7 天 6 夜行程為範例，透過 Prompt 迭代式產出可互動的旅行規劃網站
2. **ADO 工作項目視覺化** — 將 Azure DevOps 工作項目 CSV 轉換為心智圖、時間軸等 ADO/Power BI 做不到的互動式圖表

---

## 檔案說明

| 檔案 | 內容 |
|------|------|
| [GUIDE.md](GUIDE.md) | Workshop 完整教學指南，包含 SCIB Prompt 架構、7 種 Prompt 策略、反模式、漸進式教學流程 |
| [MINDMAP_PROMPT.md](MINDMAP_PROMPT.md) | ADO 工作項目互動式心智圖的完整 SCIB Prompt（含規格、節點外觀、互動行為、範例 CSV） |
| [DIAGRAM_COMPARE.md](DIAGRAM_COMPARE.md) | 5 種視覺化方案的效益比較表（依工程師 / EM / PM 三種角色分析）及互補關係圖 |
| [CONVERSATION.md](CONVERSATION.md) | 完整的討論過程回顧（13 輪迭代），記錄從構想到最終方案的決策歷程 |
| [SINGAPORE.md](SINGAPORE.md) | 新加坡 7 天 6 夜完整旅遊行程（含交通、花費、住宿選項），作為旅行網站的輸入素材 |
| [EXAMPLE.csv](EXAMPLE.csv) | ADO 工作項目範例 CSV（41 欄位、3 區段：Basic Info / Relationships / Timeline） |

---

## SCIB Prompt 架構

Workshop 核心教學的 Prompt 框架，幫助學員撰寫結構化的 GitHub Spark Prompt：

| 要素 | 說明 | 範例 |
|------|------|------|
| **S** — Structure | 頁面有哪些區塊？怎麼排版？ | 時間軸 / 卡片 / Tab / 側欄 |
| **C** — Content | 放什麼資料？給具體範例 | 景點名、時間、費用、地址 |
| **I** — Interaction | 使用者能做什麼？ | 點擊展開 / 勾選 / 拖拽 / 篩選 |
| **B** — Brand | 什麼視覺風格？ | 配色、字體感覺、參考風格 |

---

## 視覺化方案

針對 ADO 工作項目，產出 ADO 與 Power BI 無法達成的 5 種互動式視覺化：

```
Mind Map (結構) ──→ "這棵樹長什麼樣？"
     │
     ▼
Animated Timeline (時間) ──→ "這棵樹怎麼長出來的？"
     │
     ▼
Force Graph (關係) ──→ "樹上的人和節點怎麼連結？"
     │
     ▼
Treemap (資源) ──→ "每根樹枝佔了多少資源？"
     │
     ▼
Calendar Heatmap (節奏) ──→ "哪些日子在澆水施肥？"
```

---

## Workshop 流程

| 步驟 | 時間 | 內容 |
|------|------|------|
| Step 1 | 5 min | 破冰 — 展示最終成品 |
| Step 2 | 5 min | 介紹 GitHub Spark 與 SCIB Prompt 架構 |
| Step 3 | 10 min | 第一個 Prompt — 產生基礎版網站 |
| Step 4 | 15 min | 迭代改進 — 追加互動功能 |
| Step 5 | 15 min | 個人化挑戰 — 獨立操作與創意發揮 |
| Step 6 | 10 min | 分享與討論 — 比較不同 Prompt 策略的產出差異 |

---

## 前置需求

- GitHub 帳號（可存取 [GitHub Spark](https://githubnext.com/projects/github-spark)）
- 瀏覽器（建議 Chrome / Edge）
- 無需安裝任何開發工具

---

## 授權

本專案僅供 AHSI Learning Day 內部教學使用。
