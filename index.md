# 🎛️ Command Center

${widgets.commandButton("✨ New Object", "New Object")}

---

## 📓 Journal
${widgets.commandButton("➕ New Entry", "New Journal Entry")}

**Recent Entries:**
${table.concat(query[[
  from index.tag "daily"
  where name != "templates/daily_note"
  order by name desc
  limit 3
  select "* [[" .. name .. "]]"
]], "\n")}

---

## 📥 Inbox (Needs Filing)
${table.concat(query[[
  from p = index.tag "page"
  where not string.find(p.name, "/") 
  and p.name != "index" 
  and p.name != "CONFIG" 
  and p.name != "SETTINGS" 
  and p.name != "PLUGS"
  and p.name != "projects"
  and p.name != "resources"
  and p.name != "zettelkasten"
  and p.name != "archive"
  and p.name != "journal"
  and p.name != "templates"
  select "* [[" .. p.name .. "]]"
]], "\n")}

---

## 🧭 Navigation
| | |
|---|---|
| **[[projects]]** | **[[resources]]** |
| **[[zettelkasten]]** | **[[archive/]]** |

---

## ⚡ Active Work
${table.concat(query[[
  from p = index.tag "page"
  where p.name ~= "index" and not string.find(p.name, "^archive/") and not string.find(p.name, "^journal/")
  order by p.lastModified desc
  limit 5
  select "* [[" .. p.name .. "]]"
]], "\n")}
