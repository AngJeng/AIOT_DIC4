# DIC4 過程報告：用 Agent + Overleaf MCP 依 OpenSpec 修改論文

> 本報告只記錄實際發生過的事。

## 1. 交付物

| 交付物 | 位置 |
|---|---|
| 修改前 PDF | [paper/pdf/DIC4_before.pdf](paper/pdf/DIC4_before.pdf) |
| 修改後 PDF | [paper/pdf/DIC4_after.pdf](paper/pdf/DIC4_after.pdf) |
| OpenSpec 資料夾 | [openspec/](openspec/)，change 在 [openspec/changes/revise-paper-for-submission/](openspec/changes/revise-paper-for-submission/) |
| 修改前 LaTeX 原始檔 | [paper/before/main.tex](paper/before/main.tex)、[paper/before/refs.bib](paper/before/refs.bib) |
| 修改後 LaTeX 原始檔 | [paper/after/main.tex](paper/after/main.tex)、[paper/after/refs.bib](paper/after/refs.bib) |
| 逐章節差異與 Requirement 對照 | [paper/CHANGES.md](paper/CHANGES.md) |
| 文獻查證清單 | [refs-verification.md](openspec/changes/revise-paper-for-submission/refs-verification.md) |
| GitHub repo | https://github.com/AngJeng/AIOT_DIC4 |

## 2. 流程總覽

```
提案 PDF ──► baseline (before) ──► OpenSpec 規劃 ──► Agent + Overleaf MCP 修改 ──► after 版本
            (未使用 MCP)         proposal / specs /    refs.bib → Related Work →
                                 design / tasks        Introduction → Abstract
```

1. **產生 baseline（未使用 MCP）**：依 `main.tex` 檔頭的說明，baseline 是把 [提案 PDF](docs/) 的 12 頁投影片翻成英文，原樣轉成 IEEEtran 會議論文格式，Abstract、Introduction 和 Related Work 刻意沒有潤飾。修改前的 PDF 是作者從 Overleaf 專案「AIOT_DIC4_before」手動編譯下載的。
2. **OpenSpec 規劃**：建立 change `revise-paper-for-submission`，內容包括：
   - [proposal.md](openspec/changes/revise-paper-for-submission/proposal.md)：說明為什麼要改、改什麼。
   - [specs/](openspec/changes/revise-paper-for-submission/specs/)：abstract、introduction、related-work 三份 spec，每條 Requirement 都附有可檢查的 Scenario。
   - [design.md](openspec/changes/revise-paper-for-submission/design.md)：D1 保留 baseline；D2 以 Overleaf 為編輯和編譯環境，修改順序是 refs.bib → Related Work → Introduction → Abstract；D3 文獻查證流程；D4 各節結構。
   - [tasks.md](openspec/changes/revise-paper-for-submission/tasks.md)：6 組工作項目。
3. **Agent + Overleaf MCP 修改**：Claude Code 透過 MCP 直接編輯 Overleaf 專案「AIOT_DIC4_after」（原名是論文標題，作者要求後改名）。一次只改一節，每節都等作者確認才繼續。
4. **After 版本**：用 `read_file` 把最終檔案存回 `paper/after/`，用 `download_compile_output` 下載 PDF，再跑 spec 檢查和 `openspec validate`。

全部修改都在 2026-11-25 完成。

## 3. 工具與環境

| 工具 | 版本或說明 |
|---|---|
| Claude Code | VS Code extension；模型為 Claude Opus 5.5 |
| OpenSpec | 2.0.0（`openspec --version`），schema 為 `spec-driven` |
| overleaf-web-mcp | 1.4.1，用 `npx -y overleaf-web-mcp serve` 啟動（stdio） |
| Overleaf | 免費方案，編譯器 pdflatex，映像 TeX Live 2026.1 |
| 本機 | macOS，沒有安裝 LaTeX，所有編譯都在 Overleaf 上進行 |

**MCP 的選擇**：原本嘗試使用 git-based 的 Overleaf MCP，但 Overleaf 的 Git integration 需要付費方案，所以改用 session-based 的 overleaf-web-mcp。它透過登入 session 呼叫 Overleaf 的 web API，免費方案也能用。

## 4. 使用的 MCP 工具與用途

| 工具 | 用途 |
|---|---|
| `list_projects` | 找到要修改的專案及其 id；確認 baseline 專案「AIOT_DIC4_before」存在，之後沒有再碰它 |
| `get_project_tree` | 列出專案檔案（main.tex、refs.bib）、root document、編譯器 |
| `get_sections` | 解析 main.tex 的章節結構，取得 `write_section` 需要的 sectionId |
| `get_section_content` | 改寫前先讀出該節原文，例如保留 Table I |
| `read_file` | 讀取完整檔案和 revision。開始時比對 SHA-256，確認 Overleaf 上的版本和本機 baseline 相同；結束時把最終檔案存回 `paper/after/` |
| `write_section` | 改寫 Related Work 和 Introduction 這兩個整節 |
| `write_file`（局部 `edits`） | 只替換指定文字，不整檔覆寫。用在：refs.bib 寫入 9 筆文獻、取消註解 bibliography 兩行、9 個 `\label`、Abstract、Introduction 的兩處措辭修正、兩處檔頭或註解。每次送出前都先列出原文和新文字給作者確認 |
| `compile_project` | 每次修改後編譯，看 `builtCleanly`、undefined citations 和 undefined references |
| `get_compile_log`（`kind: bibtex`） | 讀 BibTeX log，確認沒有錯誤或 warning、9 筆文獻都被使用 |
| `download_compile_output` | 下載最終 PDF 到 `paper/pdf/DIC4_after.pdf` |
| `manage_project`（只用 `rename`） | 作者要求把專案改名為 AIOT_DIC4_after。這是唯一一次用到 `manage_project`，沒有用到其他 action |

**刻意不用的破壞性工具**：`delete_entities`、`manage_entity`（delete）、`sync_directory`、`manage_project` 的 trash / archive / delete，以及會覆蓋整個專案的上傳或匯入（`upload_file`、`batch_upload`、`import_project_zip`）。baseline 專案「AIOT_DIC4_before」從頭到尾沒有被讀取或寫入。

**為什麼 Abstract 不用 `write_section`**：Abstract 寫在 `\begin{abstract}` 環境裡，不是 `\section`，`get_sections` 抓不到它。bibliography 那兩行和 `\label` 也位在其他章節的範圍裡，用 `write_section` 就得重送整節。經作者同意，這些地方改用 `write_file` 的局部 edits。

## 5. 人工把關

1. **文獻查證**：每篇候選文獻的作者、標題、出處和年份，都用 Crossref API（`api.crossref.org/works/<DOI>`）或官方頁面核對過，查證清單交給作者確認後才寫入 refs.bib。Neves et al.（arXiv:1603.01367）因為查不到它聲稱的正式出處（Persuasive 2016）而被排除。作者可以在 [refs-verification.md](openspec/changes/revise-paper-for-submission/refs-verification.md) 的 "verified by user" 欄位逐筆勾選確認。
2. **每節修改前後都要確認**：Related Work、Introduction 和 Abstract 都是一節改完、回報修改內容和對應的 Requirement，等作者確認後才繼續。所有 `write_file` 的局部 edits 都是先列出原文和新文字，作者同意才送出。
3. **修正的具體例子**：
   - **Popkin 那句過強的因果宣稱**：第一版寫 "hydration status *affects* physical performance, cognitive function, and *several chronic conditions*"。作者要求只保留論文實際支持的內容，於是改成摘要範圍內的 "variation in water intake has been examined in relation to energy intake, body weight, and physical and cognitive performance"。
   - **WaterCoaster 引用自相矛盾**：第一版拿 WaterCoaster 去支持「自動系統只測量、不幫使用者行動」，但 WaterCoaster 本身就有遊戲化的鼓勵機制。作者指出這個矛盾後，我用 pypdf 從作者 preprint 抽出全文，找到提醒是怎麼觸發的："If the calculation of the current intake level drops below 70%, the smartphone triggers blue flashing of the LEDs."。句子改成依據這段原文寫：WaterCoaster 的提醒由攝取量門檻觸發，而且論文沒有描述使用運動或飲食情境。
   - **沒有直接佐證的概括句**："Many people still log their drinks by hand" 改成 "Intake tracking often relies on manual self-reporting~\cite{cohen2021fluid}"。
4. **BibTeX 驗證的順序**：剛寫入 refs.bib 時，正文還沒有任何 `\cite`，BibTeX 不會執行，所以那次編譯不算驗證過 refs.bib。我照實回報，等 Related Work 加入引用、bibliography 取消註解後，才真正跑過 BibTeX。

## 6. 修改前後摘要

| 項目 | Before | After |
|---|---|---|
| PDF 頁數 | 3 | 4 |
| refs.bib 條目數 | 0 | 9（每筆都有 DOI 或官方 URL） |
| 正文引用的文獻數 | 0 | 9 |
| Abstract | 約 134 字，以 "This is an AIoT final project proposal" 開頭 | 233 字，依「問題 → 缺口 → 方法 → 計畫中的評估 → 貢獻」排列，沒有任何數字 |
| Introduction | 問句開頭、條列寫法、有佔位字、有 Objectives 子節 | 段落寫法並附引用、RQ1–RQ3 列成清單、5 項預期貢獻、章節導覽 |
| Related Work | 一段重複 Introduction 的條列句，加上佔位字 | 3 個分類子節、比較表、Research Gap |
| 編譯 | — | builtCleanly，0 錯誤、0 警告，沒有 undefined citation 或 reference，PDF 裡沒有 "??" |

逐章節的差異和 Requirement 對照見 [paper/CHANGES.md](paper/CHANGES.md)。

**驗證結果**：
- `openspec validate --all --strict`：1 passed, 0 failed。
- 三份 spec 的 scenario 用腳本逐條檢查：27/27 PASS，紀錄在 [tasks.md](openspec/changes/revise-paper-for-submission/tasks.md) 的 6.2。

## 7. 限制

- **沒有實驗數據**：論文所有結果都是 planned 或 expected。系統還沒實作，沒有收集任何資料，也沒有任何量測結果。After 版本只改了寫法和結構，沒有新增實驗、數字或圖表。
- **範圍外的章節沒改**：System Architecture 到 Conclusion 的內容維持 baseline（只加了 `\label`）。後面的 Expected Contributions 一節和 Introduction 的貢獻清單有部分重複，依 design D5 暫時保留。
- **文獻查證的深度不一**：WaterCoaster 讀過全文，其他文獻大多只核對到 metadata 和摘要。作者的逐筆人工確認尚未完成（見第 5 節）。
- **refs.bib 檔頭的前半段**還是 baseline 的舊註解（"before" baseline、"contains NO references"），不影響編譯和 PDF。
- **OpenSpec change 尚未封存**：要等文獻全部人工確認後，才會執行 archive。
