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
## Shopping List
- [x] 3 pack Sleeveless vest white (7)
- [x] 3 pack Sleeveless vest black (7)
- [x] Long sleeved sleep shirts (4) swap
- [x] Performance pants (16)
- [ ] [M&S easy iron long sleeve shirts white 3 pack](https://www.marksandspencer.com/3-pack-regular-fit-easy-iron-shirts/p/clp60287451#intid=pid_pg1pip48g2r2c1) (49)
- [ ] [M&S easy iron long sleeve shirts black 3 pack](https://www.marksandspencer.com/3pk-regular-fit-long-sleeve-shirts/p/clp60576720#intid=pid_pg1pip48g2r11c1) (49)
- [x] Bag duffel
- [ ] Call the council for a disposal
- [ ] CDH governance check
- [ ] water bottle in green
- [ ] Beet
- [ ] ginger/tigernuts/date
- [ ] pineapple/watermelon/bananas
- [ ] coconut water
- [ ] 
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
