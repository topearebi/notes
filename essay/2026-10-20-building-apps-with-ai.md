---
title: Building Apps with AI
date: 2026-10-20 21:44 +0100
tags: [AI]
---

To reliably vibe code software, certain techniques, tools and processes can be employed to ensure that
  a. The build process is cost efficient
  b. The output is reliable

The essence of these tools and techniques is context settings. Ensuring that before you build, your Agent or Assistant has a context that has been scoped out clearly and definitively. This is intuitive because the key risk of an LLM is context drift. Or hallucination. Their scope of knowledge is so wide that when they are to be used for specific work, you must constrain what resources and directions they are able to employ/deploy in solving the set problem. Because at any single point in time they have more access to information than you do probably across your lifetime, this becomes a difficult problem indeed. And thus an important one.

> When prompting an AI agent to build a feature, you should specify each layer sequentially; define the data model, the API route/logic, then the UI interaction. is Data schema, backend rules and frontend components.

## 1. Write a Product Requirements Document (PRD)
The first tool to deploy in this scoping exercise is a Product Requirements Document. PRDs are good both for you and your Agent because they also help you define the boundaries and purpose of your project. If you did not have a clear picture of what you wanted to build, the structure of a PRD draws it out and forces you to make decisions that will come to shape your agent's context.

There are many ways to write a PRD. But if you are already vibe coding, almost any chatbot will will help here. All you need is a grill/interview prompt to get the model to query you about the product you want to build. Prefereably in multiple rounds. It is a good idea to spend a lot of time answering these questions so the model has plenty of context about your vision in order to create the PRD.
```
Example prompt
```
---

##  2. Define your frameworks.
Now this requires some traditional software development knowledge. Frameworks are sort of like recipes for how you make a meal. You need to define what tools you want your agent to use to build your project.
The very first layer of division of any product is its:
  a. Frontend / Client
  b. Backend / Server
  c. Database / State / Memory
  d. Infrastructure / Distribution

The frontend is the part that you and your users can see and interact with. The backend is the part the computer uses to transport and process instructions and commands you send through the frontend, to render new states and information again on the frontend.
Each of these parts can be built with a set of frameworks or languages, each with their own pros and cons.
This could be react, javascript, typescript, node etc.
Defining this rather than letting your Agent make a decision ensures that you are making a conscious choice in what tradeoffs you are willing to make to accomplish your goal with the product.

## 3. Find examples
LLMs are basically synthesis machines. They recreate information using informaton. And so it is logical that giving you Agent actual examples of what you want to build, means they are able to better build it. You can take advantage of a multimodal LLM here and send actual images of designs, screens etc that you would like to build, and have your Agent adapt those designs into your new and unique product. The fact of the matter is, there are hardly any truly unique products. And once you start using AI, its a bit too late to be principled on originality.

## 4. Helper documents
A product is essentially a collection of files. Usually, you start with an empty directory. And as you give your agent commands, it starts to create new files and sometimes groups of files that it puts in a directory. What that means essentially is that besides all the magic of LLMs, its role in this process is to Create, Read, Update and Delete documents. To this end, there are a few documents you can create in your project directory at the start of your project to define how your Agent behaves, operates and interacts with all the files to come.
Here's a list
I. PRD.md (or SPEC.md)
II. SCHEMAS.md
III. AGENTS.md
IV. TASKS.md (optional)
V. DESIGN NOTES.md (optional)

The .md filetype refers to markdown. Markdown is perhaps my favourite document type and the greatest thing ever invented. It is basically shorthand for notetaking. It lets you write documents and style them simply by appending symbols before or around them. Like html but just for document taking. Markdown lets you create information heirarchy and is perfectly human and machine readable. And it is also plaintext so markdown documents are very lightweight in terms of size. When you start vibecoding, you will come to find out that can quickly become very expensive.

**SCHEMAS.md**:  This is the app blueprint. Defines the application data structures, types and rules.
  - Field names: eg user_id, url, streak_count
  - Data types: eg string, integer, boolean
  - Rules and constraints: eg required vs optional, url format, password requirements.
This document serves as the single source of trtuth to ensure that different parts of the app use the same language and references rather than letting the AI decide as it could drift. AI agents also tend to be creative with nams for formats and that can lead to extra thinking, and thus extra tokens and costs
Prevents refactoring which can be expensive as it requires the AI to rewrite multiple files. If the schema is clear from the onset and edge cases have been considered, there will be little need for refactoring.

#### Aspects of schemas.md
**Schema format**: There are a few ways to write schemas, each with their tradeoffs
| Schema format | Best suited for | Key characteristics |
| --- | --- | --- |
| Typescript interfaces | checking data while it is being written from outside to the system. | Very readable for developers and AI agents but vanishes at runtime (while the app is actually running) |
| JSON Schema | <!-- e.g. cross-language contracts, validation of inbound API payloads at runtime --> | <!-- e.g. runtime validation, language-agnostic, verbose --> |
| Zod / Valibot | <!-- e.g. TypeScript apps needing runtime + compile-time validation --> | <!-- e.g. infers TS types, runtime validation, small bundle (Valibot) --> |

#### How to write one
<!-- Prompt: the repeatable sections every SCHEMAS.md should carry, in order.
  - Entities / tables: what objects exist.
  - Fields (per entity): name, type, required vs optional.
  - Constraints: formats, ranges, enums, uniqueness.
  - Relationships: one-to-many, foreign keys, cascades.
  - Edge cases: empty states, nullability, defaults.
-->

#### Worked example
<!-- Prompt: use the personal-data "Hub" app from 2026-10-19-hub-bugs-and-improvements.md.
  Per-person records: health, goals, habits, links, medical info.
  → turn that list into entity + field definitions as the concrete illustration.
-->

#### Failure modes (when no schema exists)
<!-- Prompt: catalogue drift you have actually observed or expect.
  - Fields renamed mid-build (streak_count vs streakCount).
  - Invented date/format conventions.
  - Extra thinking + token cost from ambiguity.
  - Multi-file refactors once the drift surfaces.
-->

### 2. AGENTS.md
<!-- Prompt: the runtime context/rules the coding agent reads before touching code.
  - Purpose: what this file does that SCHEMAS.md does not.
  - Project overview + tech stack.
  - Commands: install, run, test, lint, build.
  - Conventions: naming, file layout, commit style.
  - Do / Don't rules (guardrails against agent initiative).
  - File map: where things live.
-->

> **Boundary to state explicitly:** SCHEMAS.md = data truth. AGENTS.md = behaviour truth.

### 3. <!-- NAME THE THIRD DOCUMENT — currently missing -->
<!-- Candidates:
  - SPEC.md / PRD.md → what & why: user stories, acceptance criteria, out-of-scope.
  - TASKS.md / PLAN.md → execution order: sequenced work units.
  Recommendation: SPEC.md, since requirements engineering is this note's frame.
-->
<!-- Prompt: define its scope and its boundary against the other two. What goes in,
     what stays out, and which document an ambiguous requirement belongs to.
-->

---

## Step 2: The workflow (how the three interlock)
<!-- Prompt: sequence them and say who reads what, when.
  - Order: SPEC → SCHEMAS → AGENTS → build loop.
  - At each stage: which document the agent loads and why.
  - When each document gets revised (and what triggers a revision).
-->

---

## Step 3: Definition of done
<!-- Prompt: a checklist answering "requirements are complete when…".
  - [ ] Every entity named and typed.
  - [ ] Every constraint testable.
  - [ ] No TBD / TODO left in the three docs.
  - [ ] Edge cases enumerated.
  - [ ] Commands in AGENTS.md verified to run.
-->

---

## Step 4: Lessons learned
<!-- Prompt: running failure log — this is what makes the note original.
  | Date | Failure | Cost (tokens / refactor / time) | Fix applied |
  | --- | --- | --- | --- |
  | | | | |
-->
