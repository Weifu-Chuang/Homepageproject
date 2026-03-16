---
name: professional-markdown-profile-builder(CV or resume)
description: Generate structured professional profiles(CV or resume) in standardized executive-level Markdown format with strict formatting, emoji semantics, and unified date conventions.
---

# Wave's Professional Markdown Profile Builder

Generate executive-grade professional profiles in structured Markdown format.

This skill enforces:

- Strict section ordering
- Standardized date format
- Semantic emoji labeling
- Consistent heading hierarchy
- Executive tone
- No hallucinated data

---

## Core Principles

1. **Never hallucinate**
  - Do not invent dates, companies, metrics, certifications, or achievements.
  - If missing information, ask before generating.
2. **Ask one thing at a time**
  - Avoid compound clarification questions.
3. **Strict Markdown structure**
  - Output must follow the Markdown Schema exactly.
4. **Executive tone**
  - Use impact-driven, concise, high-clarity language.
  - Prefer strong verbs: Led, Delivered, Designed, Secured, Executed.
5. **Semantic emoji usage**
  - Emoji are category markers, not decoration.
  - Maximum one emoji per bullet header.
  - Emoji allowed primarily in “Key Skills”.
6. **Always surface AI capabilities when present**
  - When the user has AI-related skills or experience, ensure at least one explicit AI-focused item appears in the Key Skills and/or Work Experience sections.

---

## Mandatory Formatting Rules

### 1️⃣ Date Format (Strict)

Format:

```
MMM YYYY
```

Examples:

```
Dec 2023 – Present
Sep 2022 – Dec 2023
Jan 2012 – May 2012
```

Rules:

- Month must be three-letter English abbreviation.
- Use en dash ( – ) between dates.
- For ongoing roles, use `Present` as the end date.

---

### 2️⃣ Heading Hierarchy

| Level  | Usage                             |
| ------ | --------------------------------- |
| `#`    | Full Name                         |
| `###`  | Section Titles and Company + Role |

---

### 3️⃣ Section Separator

Use:

```
---
```

Between major sections only.

---

### 4️⃣ Bullet Formatting

- Use `-`
- No nested bullets unless explicitly required.
- Avoid paragraph blocks under bullets.
- Keep each bullet single-impact focused.

---

### 5️⃣ Skill Emoji Guidelines

Approved semantic examples:

- 🔬 Technical / Engineering
- 📈 Financial / Growth
- ⚡ Risk / Performance
- 🤝 Strategy / Alignment
- 💡 Innovation
- 🤖 AI / Automation
- 🧠 Leadership / Talent
- 🛡 Compliance / Governance

Do not use decorative emoji.

---

## Required Output Sections (Order Must Match)

1. Header Block
2. Professional Summary
3. Key Skills
4. Work Experience
5. Education & Certifications
6. Major Project List

---

## Workflow

### Step 1 – Collect Basic Info

- Full Name
- Headline
- Email
- Phone
- Location
- LinkedIn URL
- Value Proposition Sentence

---

### Step 2 – Collect Experience

For each role:

- Company
- Title
- Start Date (MMM YYYY)
- End Date (MMM YYYY or Present)
- 2–5 impact bullets

---

### Step 3 – Collect Skills

Each skill must include:

- Emoji category
- Skill title
- One impact-focused sentence

---

### Step 4 – Validate Before Output

Check:

- Date format correct
- Heading hierarchy correct
- Required sections present
- No missing placeholders
- No hallucinated content

---

## Output Rule

The final output must (and **must strictly follow** the schema defined in `references/schema.md`):

- Follow the Markdown Schema defined in `references/schema.md` exactly
- Contain no explanatory text
- Contain no JSON
- Contain no commentary
- Be pure Markdown only
- Output file name to be in camelcasing

```
END OF SKILL
```