This guide serves as the **Administrator's Manual** for your SilverBullet v0.1 instance. It documents the architecture, code, and procedures required to maintain, rebuild, or expand the system.

I recommend saving this content as a page in your system (e.g., `System/Manual`) so it is always accessible.

---

# 📘 SilverBullet System v0.1: Admin Manual

## 1\. System Architecture

This system is built on the **"Stream & Objects"** philosophy to minimize friction on mobile while maintaining structure.

*   **The Stream (Time-based):** Captured in **Daily Notes**. Used for chats, fleeting thoughts, and logging. No filing required.
    
*   **The Objects (Topic-based):** Captured as **Standalone Files** via the "New Object" automation.
    
    *   **Meetings:** Prefix `Mtg -` (Status: Open/Closed)
        
    *   **Drafts:** Prefix `Draft -` (Status: Idea/Draft/Final)
        
    *   **Poems:** Prefix `Poem -`
        
    *   **Zettels:** Prefix `ID -` (Stored in `zettelkasten/`)
        
*   **The Inbox:** All Objects (except Zettels) are created at the **Root**. The Dashboard queries them until they are manually moved to their domain folders (`projects/`, `resources/`).
    

---

## 2\. Recreation Guide (Disaster Recovery)

If you wipe your instance or migrate servers, follow these steps to restore functionality.

### Step A: Restore Templates

Create the following files in the `templates/` directory.

**1\. `templates/daily_note`**

*   **Function:** Daily logging and "Yesterday's Reflection" retrieval.
    
*   **Key Logic:** Uses `{{yesterday}}` for stability and Lua `query` for retrieval.
    

Markdown

```
---
tags: daily
type: journal
---
# 🗓️ {{today}}
[[index|⬅️ Command Center]] | [[{{yesterday}}|⏮️ Yesterday]] | [[{{tomorrow}}|⏭️ Tomorrow]]

---

## 🌅 Morning Review
> **Yesterday's Closing Thought:**
> ${ msg = query[[ from s = index.tag "section" where s.page == "{{yesterday}}" and s.name == "🌑 Evening Reflection" select s.content ]] if #msg > 0 then return msg[1] else return "(No reflection found.)" end }

## 📝 The Log
- [ ] 

## 🌑 Evening Reflection
* **Win:** * **Improvement:** * **Closing Thought:**
```

**2\. `templates/new_meeting`**

Markdown

```
---
tags: [meeting, object]
status: open
date: {{today}}
---
# Mtg - 
**Attendees:** **Context:** ---
## 🗣️ Discussion
* ## ✅ Actions
- [ ] 
```

**3\. `templates/new_writing`** (Used for Drafts & Poems)

Markdown

```
---
tags: [writing, object, draft]
status: idea
---
# Draft - 
> **Core Idea:** ---
## ✍️ Content
```

**4\. `templates/new_zettel`**

Markdown

```
---
tags: [zettel, object]
type: concept
date: {{today}}
---
# 🧠 
**Related:** [[zettelkasten/]]
---
## Concept
```

### Step B: Restore Automation

This Library page powers the "New Object" button.

**File:** `Library/Personal/ObjectCreator`

**Content:**

Markdown

````
---
name: Library/Personal/ObjectCreator
tags: meta/library
description: Automates object creation.
---
# Object Creator
```space-lua
command.define {
  name = "New Object",
  run = function()
    local options = {
      {name = "Meeting", description = "New Meeting", id = "meeting"},
      {name = "Writing Draft", description = "New Draft", id = "writing"},
      {name = "Poem", description = "New Poem", id = "poem"},
      {name = "Zettel", description = "New Zettel", id = "zettel"}
    }
    local selected = editor.filterBox("Create Object", options)
    if not selected then return end

    local title = editor.prompt("Title for " .. selected.name)
    if not title then return end

    local finalPath, templatePath = "", ""
    local today = os.date("%Y-%m-%d")

    if selected.id == "meeting" then
        finalPath = "Mtg - " .. title
        templatePath = "templates/new_meeting"
    elseif selected.id == "writing" then
        finalPath = "Draft - " .. title
        templatePath = "templates/new_writing"
    elseif selected.id == "poem" then
        finalPath = "Poem - " .. title
        templatePath = "templates/new_writing"
    elseif selected.id == "zettel" then
        finalPath = "zettelkasten/" .. os.date("%Y%m%d%H%M") .. " - " .. title
        templatePath = "templates/new_zettel"
    end

    local success, templatePage = pcall(space.readPage, templatePath)
    local content = success and templatePage and string.gsub(templatePage.text, "{{today}}", today) or "# " .. title
    
    space.writePage(finalPath, content)
    editor.navigate(finalPath)
  end
}
````

*Action:* Run `System: Reload` after saving.

### Step C: Configuration

Update `SETTINGS.md`:

YAML

```
dailyNoteTemplate: "templates/daily_note"
```

---

## 3\. Expansion Guide (Adding Functionality)

### Scenario: Adding a "Project" Object

You want to add a button to create "Project" files that go straight to `projects/` with a specific template.

**1\. Create the Template**

Create `templates/new_project` with your desired structure (Deadlines, Stakeholders, etc.).

**2\. Update the Automation (Library)**

Edit `Library/Personal/ObjectCreator`.

*   **Add to `options` table:**
    
    `{name = "Project", description = "New Project", id = "project"},`
    
*   **Add to `elseif` logic:**
    
    Lua
    
    ```
    elseif selected.id == "project" then
        finalPath = "projects/" .. title
        templatePath = "templates/new_project"
    ```
    
*   **Reload:** Run `System: Reload`.
    

**3\. Update the Dashboard Query**

If you want these to show up in the "Inbox" (Root) before filing, ensure the path logic matches. Since projects usually go straight to their folder, you might **not** need to update the Inbox query. However, you should check `projects/index.md` to ensure it picks up the new files (it should automatically, as it scans the folder).

---

### Scenario: Creating a New Hub (e.g., "Recipes")

You want a dedicated section for `resources/recipes`.

**1\. Create the Folder Structure**

Create a new note: `resources/recipes/index` (or just `resources/recipes/`).

**2\. Add the Dashboard Query**

Paste this into your new note to list all recipes:

Markdown

```
# 🍳 Recipes
${table.concat(query[[
  from p = index.tag "page"
  where string.find(p.name, "^resources/recipes/") and p.name ~= "resources/recipes/index"
  order by p.name
  select "* [[" .. p.name .. "]]"
]], "\n")}
```

---

## 4\. Troubleshooting & Maintenance

*   **Query Error `[object Object]`**: You forgot `table.concat`. Queries return tables; you must flatten them to strings.
    
*   **Query Error `Parse error`**: You likely used single equals `=` in a `where` clause (use `==`) or tried to use SQL keywords like `asc` (which Lua doesn't support—just omit it for ascending).
    
*   **Button doesn't work**: Did you run `System: Reload`? SilverBullet only compiles Libraries on boot or reload.
    
*   **"Yesterday's Reflection" is empty**: Ensure yesterday's note exists and the header is exactly `## 🌑 Evening Reflection` (emojis matter!).