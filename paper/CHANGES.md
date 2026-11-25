# CHANGES：before vs after

- **Before**：[before/main.tex](before/main.tex)、[before/refs.bib](before/refs.bib)、[pdf/DIC4_before.pdf](pdf/DIC4_before.pdf)
- **After**：[after/main.tex](after/main.tex)、[after/refs.bib](after/refs.bib)、[pdf/DIC4_after.pdf](pdf/DIC4_after.pdf)
- **Spec 來源**：OpenSpec change [`revise-paper-for-submission`](../openspec/changes/revise-paper-for-submission/)，分成 [abstract](../openspec/changes/revise-paper-for-submission/specs/abstract/spec.md)、[introduction](../openspec/changes/revise-paper-for-submission/specs/introduction/spec.md)、[related-work](../openspec/changes/revise-paper-for-submission/specs/related-work/spec.md) 三份 spec。

## 總覽

| 項目 | Before | After |
|---|---|---|
| PDF 頁數 | 3 | 4 |
| `refs.bib` 條目數 | 0 | 9 |
| 正文中被引用的文獻數 | 0 | 9 |
| References 段落 | 無（`\bibliography` 被註解掉） | IEEEtran 格式，共 9 筆 |
| Abstract 字數 | 約 134 | 233 |
| Introduction 字數（不含表格，約略） | 約 197 | 約 497 |
| Related Work 字數（不含表格，約略） | 約 48 | 約 462 |
| `\label{sec:…}` 數量 | 0 | 11 |
| `main.tex` 的 diff | — | 79 行新增、39 行刪除 |

System Architecture 到 Conclusion 這幾節的內文完全沒動，只在每個 `\section{…}` 那一行後面加上 `\label`。標題、作者欄和 keywords 也維持原樣。

---

## 1. Abstract

| | Before | After |
|---|---|---|
| 開頭 | "This is an AIoT final project proposal for a smart hydration monitoring system." | "Adequate hydration matters for health and daily performance, yet tracking how much a person drinks still often depends on manual self-reporting, …" |
| 結構 | 先列硬體，再講方法，最後列貢獻，沒有交代問題和缺口 | 問題 → 缺口 → 方法 → 計畫中的評估 → 預期貢獻 |
| 課程用語 | 有 "final project proposal" | 無 |
| LINE | 沒有定義 | "LINE, an instant-messaging application" |
| 評估 | 只寫 "All experiments and results in this paper are planned" | 用 "We plan to compare…" 寫出兩組比較和指標，並加上 "no measured results are reported yet" |
| 數字 | 有（"Raspberry Pi 4"） | 沒有任何數字（F1 改寫成 "F-score"） |

**對應的 Requirement（specs/abstract）**

| Requirement | 對應的修改 |
|---|---|
| Abstract length | 單一段落，233 字，在 150–250 範圍內 |
| Abstract content order | 第 1 句寫問題，第 2 句寫缺口，第 3–6 句寫方法，第 7 句寫評估，最後一句寫貢獻 |
| Abstract makes no unsupported result claims | 沒有數字、%、outperform、improve；評估一律寫成計畫中 |
| Abstract uses paper register | 拿掉 "final project"、"proposal"；第一次出現 LINE 時就加上定義 |

---

## 2. Introduction

| | Before | After |
|---|---|---|
| 第一段 | 用問句開頭："Why do we need smart hydration management?" | 用敘述句說明 hydration 和健康的關係，並引用 `\cite{popkin2010water}`、`\cite{cohen2021fluid}` |
| 現況的限制 | 以分號串起來的條列句，沒有引用 | 以段落說明手動記錄、只做測量的系統、規則式提醒（WaterCoaster、Playful Bottle），以及 JITAI 的觀點，每句都有引用 |
| 動機 | "Research motivation: …"（投影片標籤寫法） | 刪除，改成一段說明 IoT、ML、LLM 怎麼讓新做法可行，並概述系統 |
| 佔位字 | "[To be added: 2025--2026 public research …]" | 刪除 |
| "this project" | 有 | 無 |
| RQ | 只出現在 Table I 裡 | 正文用 `enumerate` 列出 RQ1–RQ3，Table I 原樣保留並在正文引用 |
| Objectives 子節 | 4 項 objectives | 刪除，換成 "The expected contributions of this paper are:" 加上 5 項預期貢獻（每項都寫成 design、plan 或 planned） |
| 章節導覽 | 沒有 | 最後一段用 10 個 `\ref` 依序介紹後面各節 |
| 子節標題 | Research Questions、Objectives | 拿掉，讓 Introduction 連成完整的段落 |

**對應的 Requirement（specs/introduction）**

| Requirement | 對應的修改 |
|---|---|
| Motivation in prose with citations | 第 1–3 段改成段落寫法，所有事實陳述都有 `\cite`，也沒有問句開頭和標籤 |
| Three research questions stated explicitly | RQ1–RQ3 在正文各出現一次，意思和原本的 Table I 一致 |
| Contributions list | 5 項，取自原本的 Expected Contributions，沒有過度宣稱 |
| Paper organization paragraph | 導覽段落裡的 10 個 `\ref` 全部能解析，編譯後沒有 "??" |
| No placeholders or course framing | 刪除 "[To be added"、"this project"、"Research motivation:" |

**經過人工修正的兩句**
- **Popkin 那句：** 第一版寫成 "hydration status affects physical performance, cognitive function, and several chronic conditions"，因果語氣太強，也超出摘要能支持的範圍。修正後是 "variation in water intake has been examined in relation to energy intake, body weight, and physical and cognitive performance"。
- **手動記錄和 WaterCoaster：** 第一版寫 "Many people still log their drinks by hand"，這句沒有直接佐證。而且原本拿 WaterCoaster 來支持「自動系統只測量、不幫使用者行動」，但 WaterCoaster 本身就有遊戲化的提醒，引用互相矛盾。修正後改成 "manual self-reporting `\cite{cohen2021fluid}`"，並根據 WaterCoaster 論文全文，描述它的提醒是由攝取量門檻觸發的。

---

## 3. Related Work

| | Before | After |
|---|---|---|
| 內容 | 一段和 Introduction 重複的條列句，加上 "[To be added …]" | 開頭一段總說，接著 5 個子節 |
| 分類 | 沒有 | A. Sensor-Based Fluid Intake Monitoring（cohen2021、lester2010、lessel2016）<br>B. Hydration Reminders and Just-in-Time Interventions（chiu2009、lessel2016、nahumshani2018）<br>C. LLM- and Agent-Based Health Assistants（singhal2023、kim2024、yao2023） |
| 比較 | 沒有 | Table `tab:rw`：4 個維度（Sensing、Auto. intake、Context、Adaptive reminders）。最後一列標為 "Proposed (design)"，寫的是設計特性 |
| 研究缺口 | 沒有 | "However, existing approaches…"、"To the best of our knowledge, no prior work…"，最後連到 RQ1–RQ3 |
| 引用 | 0 | 8 篇（popkin2010 只在 Introduction 引用） |

**對應的 Requirement（specs/related-work）**

| Requirement | 對應的修改 |
|---|---|
| Categorized discussion of prior work | 3 個類別子節，每類引用 ≥2 篇 |
| Comparison with the proposed system | Table `tab:rw` 的 4 個維度；本系統那一列寫成設計特性，並在正文說明 "not measured results" |
| Explicit research gap | 最後一個子節 Research Gap 有明確的缺口敘述，並對應到 RQ1–RQ3 |
| Only real, verifiable citations | 9 筆都對照 Crossref 或官方頁面查證過，清單見 [refs-verification.md](../openspec/changes/revise-paper-for-submission/refs-verification.md) |
| Citations compile cleanly | `\bibliographystyle{IEEEtran}` 生效；編譯沒有 undefined citation，BibTeX 沒有 warning |
| No duplication or placeholders | 刪除和 Introduction 重複的句子以及 "[To be added" |

---

## 4. 其他修改（為上面三節服務）

| 位置 | Before | After | 原因 |
|---|---|---|---|
| 後面 9 個 `\section` | `\section{System Architecture}` 等 | `\section{System Architecture}\label{sec:architecture}` 等 | 讓 Introduction 的章節導覽可以 `\ref`（task 4.1） |
| Bibliography | `% \bibliographystyle{IEEEtran}`、`% \bibliography{refs}`（被註解掉） | 取消註解 | Citations compile cleanly（task 2.3） |
| Bibliography 上方的註解 | 寫著 "refs.bib is therefore empty …" | 改成指向 refs-verification.md | 舊註解已不符合現況 |
| 檔頭註解 | `DIC4 -- "BEFORE" baseline version …` | `DIC4 -- "AFTER" version`，並說明修改流程和範圍 | 標示這是 after 版本 |
| `refs.bib` | 只有註解，沒有條目 | 加入 9 筆查證過的條目（檔頭前半段仍是 baseline 的註解） | Only real, verifiable citations |
