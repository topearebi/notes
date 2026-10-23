# Personal Vault Architecture & Schema Standards

## File Structure & Frontmatter
- Daily morning briefs MUST be written to `morning-briefs/YYYY-MM-DD-morning-brief.md`.
- Every brief must begin with standard YAML frontmatter:
  ```yaml
  ---
  title: Daily Brief — YYYY-MM-DD
  date: YYYY-MM-DD
  tags: [brief, executive, ops]
  summary: 2-sentence executive summary of the day.
  ---

```

* Section headers must strictly follow:
* `## 1. Actionable Intelligence` (Tools, frameworks, and practical triggers)
* `## 2. Armature Ops & Commercial Loops` (Status, proposals, blockers)
* `## 3. Personal Habits & Logistics` (Cutoffs, training, languages)
* `## 4. Top 3 Concrete Decisions For Today`


* Action items MUST strictly use GitHub task checkboxes: `- [ ]` for pending, `- [x]` for complete.

```
