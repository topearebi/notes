---
tags: [daily, journal]
type: journal
---
# 📓 Title

[[index|🏠 Command Center]]

---

## ⚡ The Stream
- [ ] 

---

## 🧠 Thought Blocks
### 💡

### 💡
### 
💡

---

## 🌑 Reflection

### 😤 Friction & Negatives
- 
### 🌟 Wins & Positives
- 
### 🧠 Learnings
*
---
**Recent Entries:**
${table.concat(query[[
  from index.tag "daily"
  where name != "{{name}}" and name != "templates/daily_note"
  order by name desc
  limit 5
  select "* [[" .. name .. "]]"
]], "\n")}
