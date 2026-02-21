---
name: Library/Personal/ObjectCreator
tags: meta/library
description: Unified creator for Meetings, Drafts, Zettels, and Journals.
---

# Object Creator

```space-lua
-- SHARED LOGIC: The core creation function
local function create_object(forcedType)
    -- 1. Configuration
    local options = {
      {name = "Journal Entry", description = "Format: 001 - Date", id = "journal", template = "templates/daily_note"},
      {name = "Meeting", description = "New Meeting Notes", id = "meeting", template = "templates/new_meeting"},
      {name = "Writing Draft", description = "Essay/Article Draft", id = "writing", template = "templates/new_writing"},
      {name = "Zettel", description = "Atomic Note", id = "zettel", template = "templates/new_zettel"}
    }

    local selected = nil

    -- 2. Determine Type (Menu vs Direct)
    if forcedType then
        -- Find the option that matches the forced ID
        for _, opt in ipairs(options) do
            if opt.id == forcedType then
                selected = opt
                break
            end
        end
    else
        -- Show the menu
        selected = editor.filterBox("Create Object", options)
    end

    if not selected then return end

    -- 3. Input Title
    local promptText = "Title"
    if selected.id == "journal" then
        promptText = "Journal ID (e.g., 'Feb 16 2026')"
    else
        promptText = "Title for " .. selected.name
    end

    local title = editor.prompt(promptText)
    if not title then return end

    -- 4. Calculate Path & Metadata
    local finalPath = ""
    local objectFrontmatter = ""

    if selected.id == "journal" then
        finalPath = "journal/" .. title
        objectFrontmatter = "tags: [daily, journal]\ntype: journal"
    elseif selected.id == "meeting" then
        finalPath = "Mtg - " .. title
        objectFrontmatter = "tags: [meeting, object]\nstatus: open"
    elseif selected.id == "writing" then
        finalPath = "Draft - " .. title
        objectFrontmatter = "tags: [writing, draft]\nstatus: idea"
    elseif selected.id == "zettel" then
        finalPath = "zettelkasten/" .. os.date("%Y%m%d%H%M") .. " - " .. title
        objectFrontmatter = "tags: [zettel, object]\ntype: concept"
    end

    -- 5. Read & Process Template
    local success, rawText = pcall(space.readPage, selected.template)
    local content = ""

    if success and rawText then
        -- Strip existing Frontmatter
        if string.sub(rawText, 1, 3) == "---" then
             local _, endPos = string.find(rawText, "\n%-%-%-", 4)
             if endPos then
                 rawText = string.sub(rawText, endPos + 1):gsub("^%s+", "")
             end
        end
        -- Replace generic title placeholder
        rawText = rawText:gsub("# 📓 Title", "# 📓 " .. title)
        rawText = rawText:gsub("|%^|", "") -- Remove cursor marker
        
        content = "---\n" .. objectFrontmatter .. "\n---\n" .. rawText
    else
        content = "---\n" .. objectFrontmatter .. "\n---\n# " .. title .. "\n\n(Template missing)"
        editor.flashNotification("Warning: Template read failed.", "warning")
    end

    -- 6. Create & Open
    space.writePage(finalPath, content)
    editor.navigate(finalPath)
end

-- COMMAND 1: The General Menu
command.define {
  name = "New Object",
  run = function()
    create_object(nil)
  end
}

-- COMMAND 2: Direct Journal Entry
command.define {
  name = "New Journal Entry",
  run = function()
    create_object("journal")
  end
}