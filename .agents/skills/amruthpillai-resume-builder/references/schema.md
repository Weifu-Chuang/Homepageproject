## Professional Markdown Profile Schema v1.0

This schema defines the exact output structure required by the Professional Markdown Profile Builder skill.

---

## Top-Level Structure

```
# {Full Name}

---

<aside>
💡

**{Professional Value Proposition Sentence}**

</aside>

> **{Headline | Industry | Key Metrics | Core Identity}**
>
- **Email:** {email}
- **LinkedIn:** [{display_text}]({url})
- **Phone:** {phone}
- **Location:** {city, country}

---

### Professional Summary

{3–6 sentence executive-level paragraph. No emoji.}

---

### Key Skills

- **{Emoji} {Skill Category}:** {Impact-driven description}
- **{Emoji} {Skill Category}:** {Impact-driven description}
- **{Emoji} {Skill Category}:** {Impact-driven description}

---

### Work Experience

### **{Company Name}** | {Title}

*{MMM YYYY – MMM YYYY or Present}*

- {Impact achievement}
- {Impact achievement}

(Repeat for each role)

---

### Education & Certifications

- **{University Name}:** {Degree} ({YYYY - YYYY})

- **Certifications:**
  - {Certification Name}
  - {Certification Name}

---

### Major Project List

1. **{Project Name} ({Budget or Value}):** {One-line impact summary}
2. **{Project Name}:** {One-line impact summary}

---
```

---

## Structural Rules

### 1️⃣ Date Rule

For work experience dates, use:

```
MMM YYYY – MMM YYYY or Present
```

For education ranges, use:

```
YYYY - YYYY
```

Invalid examples:

* 2023-12-01
* 12/01/2023
* Dec 2023 - 12/2024

---

### 2️⃣ Heading Rule

* Only one `#` for the full name
* All sections use `###`
* All roles also use `###` with the pattern `### **{Company Name}** | {Title}`

---

### 3️⃣ Emoji Rule

* Required in Key Skills
* Optional in Major Projects
* Forbidden in Summary paragraph

---

### 4️⃣ Bold Usage

* Company names must be bold
* Project names must be bold
* Value proposition must be bold
* Certification titles NOT bold

---

### 5️⃣ Content Constraints

* No personal pronouns (“I”, “my”)
* No filler language
* No emojis in paragraphs
* No HTML except `<aside>`

---

### 6️⃣ Ordering Constraint

Sections must appear exactly in this order:

1. Header
2. Professional Summary
3. Key Skills
4. Work Experience
5. Education & Certifications
6. Major Project List

No deviation allowed.

---

## Optional Machine-Readable Definition

```json
{
  "version": "1.0",
  "required_sections": [
    "header",
    "professional_summary",
    "key_skills",
    "work_experience",
    "education",
    "major_project_list"
  ],
  "date_format": "MMM YYYY",
  "emoji_required_in": ["key_skills"],
  "emoji_forbidden_in": ["professional_summary"],
  "heading_levels": {
    "name": "#",
    "section": "###",
    "company": "###"
  }
}
```

