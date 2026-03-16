---

## name: anthropics-doc-coauthoring-schema  
description: Guide users through co-authoring PM documents that follow a  
  predefined schema (template). Use when user asks to "write a communication  
  plan", "create a stakeholder plan", "draft an engagement plan", "溝通計劃",  
  or "利害關係人計劃". Load the matching schema from references/, collect  
  schema-specific context, then fill sections and tables. For doc types  
  without a predefined template, use the non-schema doc co-authoring skill.

# Doc Co-Authoring — Schema-Driven Mode

This skill guides users through creating **Communication Plan** or **Stakeholder Engagement Plan** (or other schema-based PM documents) using predefined templates. The structure comes from the schema; you gather context to fill it, then iterate and run Reader Testing.

## When to Use This Skill

**Trigger conditions:**

- User asks for: "communication plan", "stakeholder plan", "stakeholder engagement plan", "engagement plan"
- User asks in 中文: "溝通計劃", "利害關係人計劃"

**Do not use** for general docs, specs, proposals, or decision docs without a template — use the **anthropics-doc-coauthoring** (no-schema) skill instead.

**Initial offer:** Tell the user this document type has a predefined structure; sections and tables will be pre-loaded from the standard schema so they don’t need to decide the structure. Then proceed to Step 1.

---

## Missing-Information Notice (use every time you ask for required or missing info)

**Whenever you ask the user for required or missing information, append this notice at the end of that request:**

- **English:** "If you don't have or aren't sure about any item, please say so explicitly (e.g. 'no data', 'TBD', 'skip'). I will use a default or assumed value and you can edit it later in the document."
- **繁體中文：** 「若某項資訊您目前沒有或不清楚，請**明確告知**（例如『沒有資料』、『待補』、『先跳過』）；系統將帶入**預設值或合理假想值**，您之後可隨時在文件內修改。」

Use the language that matches the user's.

---

## Step 1: Load the Schema

Before asking for context, read the correct schema from `references/`:

- **Communication Plan** → read `references/com-plan-schema.md`
- **Stakeholder Plan / Stakeholder Engagement Plan / Engagement Plan** → read `references/stakeholder-plan-schema.md`

If the user requests another doc type that has a schema file in `references/`, read that file and follow the same workflow below.

Inform the user: "This document type has a predefined structure. The sections and tables will be pre-loaded from the standard schema — no need to decide the structure."

---

## Step 2: Collect Schema-Specific Context (in segments)

Replace the generic "document identity" questions with **schema-specific fields**. Ask in **2–3 segments**. After **each segment**, append the **Missing-Information Notice** above. If the user says they have no data for an item (e.g. "沒有資料", "no data", "TBD"), use the **Default / assumed values** from the table below when generating the document.

**Segment A — people & project identity:**

1. Project name（專案名稱）
2. Project Manager name（專案經理姓名）
3. Sponsor name（贊助者姓名）

Append Missing-Information Notice.

**Segment B — timeline & organization:**

4. Project duration (e.g. MMM YYYY – MMM YYYY)（專案期間）
5. Project type (Waterfall / Hybrid / Agile)（專案類型）
6. Organization type (Functional / Matrix / Projectized)（組織類型）

Append Missing-Information Notice.

**Segment C — stakeholders & channels (short info-dump):**

Ask for: main stakeholders, communication tools/channels, escalation thresholds, confidentiality needs — as relevant to the doc type. Encourage a short info-dump. Append Missing-Information Notice.

**Default / assumed values when user says "no data" or "TBD":**

| Field                | Default / assumed value                     | In document                                 |
| -------------------- | ------------------------------------------- | ------------------------------------------- |
| Project name         | 專案名稱待定 / Project name TBD              | Use as-is; user can replace later           |
| Sponsor name         | Sponsor 待定 or 專案經理之上級               | Use as-is or placeholder                    |
| Project duration     | 待定 / TBD or leave as {MMM YYYY – MMM YYYY} | Mark so user can find and edit              |
| Project type         | First option (e.g. Waterfall) or 待定        | Prefer schema default                       |
| Organization type    | First option (e.g. Matrix) or 待定           | Prefer schema default                       |
| Other missing fields | Use schema example row or "[待補]" / "[TBD]" | Clearly mark so user can search and replace |

Then proceed to Step 3.

---

## Step 3: Build Document from Schema (Schema-Driven Stage 2)

Use the **schema** as the only source for document structure. Do not ask the user what sections they need.

### Create the scaffold

- **If creating a file (no artifacts):** Ask the user for the desired path and/or filename; default is the working directory. Create the output file using the schema structure (all section headers and placeholder rows pre-populated).
- **If using artifacts:** Create the artifact with the schema structure (all section headers and placeholder rows pre-populated).

### Fill each section

Work through each section one by one:

- **When asking for missing table fields or names:** Ask in small batches (e.g. 2–4 items) and append the **Missing-Information Notice** so the user knows they can say "no data" and you will use defaults.
- **Table-based sections:** Skip full brainstorming. Propose **3–5 row entries** based on context gathered, then ask the user to confirm, edit, or add rows. If they have no data for a row or cell, use the schema example or a "[待補]" / "[TBD]" placeholder.
- **Narrative sections** (e.g. Purpose, Objectives): Follow a shortened flow — a few clarifying questions, then draft and refine with `str_replace`. No long brainstorm list required.

### Drafting and refinement

- Use `str_replace` to replace placeholders and fill content; never reprint the whole doc.
- After each edit, provide the artifact link (if using artifacts) or confirm the file path (if using files).
- Iterate until the user is satisfied with each section. After 3 consecutive iterations with no substantial changes, ask if anything can be removed without losing important information.

### Near completion

When 80%+ of sections are done, re-read the full document and check for flow, consistency, redundancy, contradictions, and filler. Provide feedback and any final suggestions. Then proceed to Step 4 (Reader Testing).

---

## Step 4: Reader Testing (Stage 3)

**Goal:** Test the document with a fresh Claude (no context) to verify it works for readers.

### If access to sub-agents is available (e.g. Claude Code)

1. **Predict reader questions:** Generate 5–10 questions readers would ask when discovering this document.
2. **Test with sub-agent:** For each question, invoke a sub-agent with only the document content and the question. Summarize what Reader Claude got right/wrong.
3. **Additional checks:** Invoke sub-agent to check for ambiguity, false assumptions, contradictions. Summarize issues.
4. **Report and fix:** If issues are found, list them and loop back to refinement for those sections.

### If no access to sub-agents (e.g. claude.ai)

1. **Predict reader questions:** Generate 5–10 questions readers would ask.
2. **Setup testing:** Instruct the user to open a fresh conversation at [https://claude.ai](https://claude.ai), paste or share the document, and ask Reader Claude the generated questions. For each, have Reader Claude give the answer, note ambiguities, and state what the doc assumes readers already know.
3. **Additional checks:** Have the user also ask Reader Claude: "What might be ambiguous?", "What does this doc assume readers already know?", "Any contradictions?"
4. **Iterate:** Based on what Reader Claude got wrong or struggled with, fix those gaps and refine the document.

### Exit condition

When Reader Claude answers questions correctly and no new gaps or ambiguities appear, the doc is ready.

---

## Final Review

When Reader Testing passes:

1. Recommend a final read-through by the user — they own the document.
2. Suggest verifying facts, links, and technical details.
3. Ask if the document achieves the impact they wanted.

If they want one more review, provide it. Otherwise, announce completion and optionally suggest: linking this conversation in an appendix, using appendices for depth, and updating the doc as real readers give feedback.

---

## Reference Files

- **`references/com-plan-schema.md`** — Communication Plan: sections, table structures, guidance notes.
- **`references/stakeholder-plan-schema.md`** — Stakeholder Engagement Plan (PMI-aligned): register, assessment, strategy tables.

To add a new schema-based doc type: add a new schema file under `references/`, then in Step 1 map the user’s request to that file and follow the same workflow (schema-specific context → scaffold from schema → fill sections/tables → Reader Testing).
