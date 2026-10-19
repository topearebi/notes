---
title: Readover bugs and improvements
date: 2026-10-18 05:48 +0100
---

- In text citations for external sources. How do the cards for those render. Not quotes this time, but URL pointers especially Wikipedia or maybe urban dictionary. Reliable go to resources.

- Deleting highlights in books. We can create them, but how would we delete them? Especially if the have attached notes. Logic to view notes by book source?

- notes from a deleted book can reference the position if the book is restored. However the highlight is not currently recreated. Is this possible?

- Adapting [[Learning Techniques]] to Readover. 
    - improve the spaced repetition algorithm
    - implement active recall. 1. A type card that acts as a flash card. Shows up in the feed, and asks a prompt or query instead of a question. Downside, user has to put in extra work for this. Could be implemented as a tag or type. Tag is cleaner and nondestructive for organisation. 2. Randomly, some cards in feed will only show titles and headings giving the reader a chance to recall what the card is about before clicking into it. 3 extract headers and inject them into the feed. So cards show up as normal, but headers and title blocks show up occasionally as prompts/questions for the user to think about. The user can tap the title to review? But would require the user creating highly structured markdown notes. How would these work for highlights or thoughts? can't ask what did you think on the 20th of september? perhaps some truncated text? 2-3 words to start off?

- Markdown viewer or markdown tools in the note editor. Quick tips or shortcuts for checkboxes, indents etc. Things that aren't normally intuitive. Lists are easy enough. Sub lists and indentation

- highlights don't show the stage pill in the feed menu, but they show it in the note view.

- the keyboard shortcut guide in settings is gone

- there's a bug with the Dropbox connection. It always creates a duplicate connection that doesn't sync

- how is conflict resolution managed. I'm connected on multiple devices and do frequent handovers. If I'm writing a note and it becomes an essay, I'll easily just move over to the laptop. They're all pointing to my GitHub but how would this be managed for all storage locations? Do we put a block on note edition if something is open? Or a note? Basically prevent conflict? Or provide a resolution step. Compare file contents or compare file meta like edit date, file size, character count etc for the user to pick the valid one? Or is it automatically balanced and duplicated?



- Readover let's you curate information once you find it
But how do you find new information? Social features? Shared zettelkastens? Finding connections across other people's repo. Temporary one time connection for shared editing. Index with tags? How do you determine what's related without existing links? Complex. Or a permanent connection for users to browse an external note and import it or make connections to their existing work. Shared human knowledge.



-- 

## Bugs
[ x ] Dark mode is broken. Across all screens. Editor and note creator are uselles

## Ideas
- I still can't think of a way to do info summaries without AI. Maybe information should not be summarised and this is more a philosophy problem than a technical one

## Questions
- Do keyboard shortcuts still work in readover editors and canvas. Can you copy, paste, highlight etc. ctrl s to save? Think of desktop usecases or a user connects their keyboard to their phone for long form essay writing. All the buttons should have keyboard equivalents if possible. Trigger to cite, link, graft, enter.

- Can you navigate the feed with a keyboard or clickker. Go up, go down, enter, edit, water, prune, archive, hide, delete etc

#pwas
