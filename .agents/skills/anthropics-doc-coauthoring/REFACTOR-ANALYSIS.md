# Skill 重構思路分析：Doc Co-Authoring 拆成「無 Schema」與「Schema 驅動」

## 一、現狀

目前單一 `anthropics-doc-coauthoring` skill 內含兩條並行路徑：


| 路徑            | 觸發條件                                      | 流程特徵                                                                                                              |
| ------------- | ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **一般流程**      | 寫文件、提案、規格、決策文件、PRD、RFC 等                  | Stage 1 通用脈絡收集 → Stage 2 由使用者與 AI 一起決定章節、腦力激盪、逐段撰寫 → Stage 3 讀者測試                                                 |
| **Schema 驅動** | 溝通計劃、利害關係人計劃、engagement plan、溝通計劃、利害關係人計劃 | 先讀取 `references/` 內對應 schema → 依 schema 收 PM 專用欄位（分段）→ 依 schema 產生預設章節/表格骨架 → 填表/填段落，表格區不做腦力激盪 → 同樣做 Stage 3 讀者測試 |


兩者共用：Missing-Information Notice、分段提問、Reader Testing、artifact/檔案操作與品質建議。差異在：**是否有預先定義的 document schema**（章節、表格欄位、預設值）決定「要不要用 schema 檔」與「Stage 1/2 的問法與產出方式」。

---

## 二、重構目標

- **Skill A（無 Schema）**：適用「沒有預設模板」的寫作情境，由使用者與 AI 共同決定結構與內容。
- **Skill B（Schema 驅動）**：適用「已有 schema 的檔案類型」（目前為溝通計劃、利害關係人計劃），依 schema 收脈絡、產骨架、填表與敘述段落。

好處：

1. **觸發更精準**：Agent 可依關鍵字/意圖選擇 A 或 B，避免在單一 skill 裡用長串 if/else。
2. **維護單純**：Schema 新增（例如未來加「風險管理計劃」）只改 B 與其 references，不影響 A。
3. **閱讀負擔小**：每個 skill 只描述一條路徑，邏輯更清楚。

---

## 三、拆分原則

### 3.1 哪些保留在「無 Schema」Skill（A）

- **觸發**：寫文件、提案、規格、決策文件、PRD、design doc、RFC 等；**不**包含「溝通計劃 / 利害關係人計劃 / 溝通計劃 / 利害關係人計劃」。
- **內容**：完整三階段（Context Gathering → Refinement & Structure → Reader Testing），包含：
  - Missing-Information Notice、分段提問（Segment 1/2/3）、info dump、澄清問句
  - Stage 2：由使用者決定或由 AI 建議 3–5 個章節 → 腦力激盪 → 精選 → 撰寫 → 迭代
  - Stage 3：Reader Testing（含 sub-agent 與無 sub-agent 兩種做法）
  - 與 artifact/檔案、品質、語氣相關的 Tips
- **移除**：整段「PM Document Templates (Schema-Driven Mode)」；在「When to Offer」可加一句：若使用者要寫的是**溝通計劃**或**利害關係人計劃**，改用 **Schema 驅動** 的 doc co-authoring skill。

### 3.2 哪些放到「Schema 驅動」Skill（B）

- **觸發**：明確要求「溝通計劃 / 利害關係人計劃 / engagement plan」或「溝通計劃 / 利害關係人計劃」。
- **內容**：
  1. **載入 schema**：依文件類型讀取 `references/com-plan-schema.md` 或 `references/stakeholder-plan-schema.md`。
  2. **Schema 專用脈絡收集**：取代通用 Stage 1 的開放式問法，改為分段收集 PM 欄位（Segment A/B/C）+ Default/assumed values 表 + Missing-Information Notice。
  3. **Schema 版 Stage 2**：以 schema 為唯一骨架來源；表格區用「提議 3–5 列 → 使用者確認/編輯/補列」取代腦力激盪；敘述段落（Purpose、Objectives）仍可保留簡化版 brainstorm → curate → draft。
  4. **Stage 3**：與 A 相同（Reader Testing），可全文寫在 B 或寫成「同標準流程」並簡要列出步驟。
- **References**：B 專用資料夾內保留 `com-plan-schema.md`、`stakeholder-plan-schema.md`，方便日後擴充其他 schema（例如新檔 `risk-plan-schema.md` 並在 B 的觸發與 Step 1 註明）。

### 3.3 重複內容的處理

- **Reader Testing**：兩邊流程相同，採**在兩份 SKILL 裡各自寫完整**，以便單獨啟用任一個 skill 時都能獨立運作。
- **Missing-Information Notice、分段提問、artifact/檔案**：兩邊都會用到，在各自 skill 內保留精簡說明，不抽成第三個「共用 skill」，以減少依賴與載入順序問題。

---

## 四、檔案與資料夾規劃


| 項目              | 無 Schema（A）                                                   | Schema 驅動（B）                                                             |
| --------------- | ------------------------------------------------------------- | ------------------------------------------------------------------------ |
| **Skill 位置**    | 沿用 `anthropics-doc-coauthoring/`                              | 新增 `anthropics-doc-coauthoring-schema/`                                  |
| **SKILL.md**    | 現有 SKILL.md 刪掉 Schema 區塊，並在觸發處註明「溝通/利害關係人計劃 → 用 schema skill」 | 新 SKILL.md：觸發、載入 schema、PM 脈絡、Schema 版 Stage 2、Stage 3                   |
| **references/** | 無需 schema 檔（可保留空或刪除）                                          | 複製 `com-plan-schema.md`、`stakeholder-plan-schema.md` 到 B 的 `references/` |


這樣 A 與 B 各自自包含，B 的 schema 擴充不影響 A，且兩者都能被 agent 依意圖個別觸發。

---

## 五、總結

- **拆分的本質**：依「**有無預先定義的 document schema**」分成兩條工作流，而不是依「文件類型名稱」硬拆。
- **無 Schema skill**：通用協作寫作，結構與內容由對話中長出來。
- **Schema 驅動 skill**：模板與欄位已定，流程是「載入 schema → 收 schema 所需脈絡 → 依 schema 建骨架 → 填表與敘述 → 讀者測試」。
- 重構後兩個 skill 可並存、觸發條件互斥且清晰，日後若要加新 schema 只需在 B 與其 `references/` 擴充。

