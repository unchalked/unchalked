# Deck Builder Prompt

**How to use (for the student):** Fill in what you already know in the YOUR REQUEST block below and leave the rest blank, then paste this whole file into an AI chat (Claude, ChatGPT, Gemini, any of them) and attach or paste your class materials right after it. The AI first asks you a few quick setup questions about whatever you left blank (mastery type, questions per concept, size, and so on), each with a recommended answer, so you can reply with something like "1b, 2a, 3: 5" or "use your recommendations". Then it builds the deck. Copy the JSON it returns into the study app's Import screen.

---

## YOUR REQUEST (student fills this in; delete the lines that do not apply)

```text
JOB: <create | add | revise | upgrade>
   create  = build a brand-new deck for a class
   add     = add a chapter or new material to a deck I already have
   revise  = fix or improve specific items in a deck I already have
   upgrade = add readings and extra questions to a deck I already have (keeps every id)
ORGANIZED BY: <STUDY GUIDE | TEXTBOOK OBJECTIVES | SYLLABUS | OTHER: describe>
   Which of my materials defines how the class is organized. The deck copies its structure:
   its sections become chapters, its items become objectives, same order, numbering and wording.
LABELS (optional): <e.g. chapter = Unit, objective = Study guide item; blank = Chapter / Objective>
MASTERY: <CONCEPT | QUESTION | blank = the AI recommends one and asks me>
   CONCEPT  = an objective is mastered after I answer 3 different questions on it right (needs 4-6 questions each)
   QUESTION = each question is mastered after I answer it right twice in a row
QUESTIONS PER CONCEPT: <e.g. 4-6; blank = the AI recommends a number and asks me (usual: 4-6 for CONCEPT, 2-3 for QUESTION)>
QUESTION STYLE (optional): <e.g. "like the exam: mostly scenarios", "include calculations", "some select-all-that-apply", "harder than the textbook"; blank = the AI asks>
SYNC + PHONE SETUP (optional): <YES | NO; blank = the AI asks when I create my first deck>
   YES = after the deck, walk me through putting my decks online, syncing progress across my devices,
         and adding them to my phone's home screen as an app (about 5 minutes; requires a free GitHub account)
   NO  = skip it. Without it, my progress still saves in each browser on each device; it just doesn't move between them
SETUP QUESTIONS: <ASK | SKIP; blank = ASK>
   ASK  = before generating, the AI asks me a few quick questions about anything in this block I left blank
   SKIP = the AI uses its recommended choices and builds the deck right away

CLASS NAME / COURSE CODE: <e.g. FIN 331 Corporate Finance>
DECK ID (slug, only for "create"; for add/revise/upgrade it comes from my existing deck): <e.g. fin331-fall2026>
TODAY'S DATE (YYYY-MM-DD): <date>
MATERIALS I AM ATTACHING: <slides / notes / textbook chapter / study guide / learning objectives>
CHAPTER(S) THESE MATERIALS COVER: <e.g. Chapter 4 - Bonds>
LEARNING OBJECTIVES: <paste them, or write "extract from the materials", or "none provided">
OUTPUT CHOICE (only for add/revise/upgrade):
   FULL = give me the complete merged deck (I will use Import > Replace)
   ADDITIONS = give me only the new/changed items (I will use Import > Merge)
CONTENT TO GENERATE: <TERMS | QUIZ | LEARN | READINGS | ALL, or a combination like "QUIZ + READINGS">
   TERMS    = flashcards only (smallest, cheapest reply)
   QUIZ     = multiple-choice questions with after-answer explanations, no pre-answer concept text
   LEARN    = multiple-choice questions that also carry pre-answer concept text (work in both Learn and Quiz mode; the longest reply)
   READINGS = a short reading for each objective, shown in Learn mode before its questions and behind "Read about this"
   ALL      = TERMS + READINGS + LEARN
   Leave blank and the AI will ask me before generating anything.
SIZE / FOCUS: <e.g. about 25 terms and 20 questions; or "as many as the material warrants"; or "focus on the formula-heavy parts"; blank = the AI estimates and asks>
GOOGLE DOC LINKS (optional): <the link of my notes doc, and any heading/bookmark links, each labelled with the topic it points to>
EXISTING DECK JSON (add/revise/upgrade only): paste it below this block, or write "none".
ITEMS TO FIX (revise only): <ids and what is wrong, or "use your judgment on the deck I pasted">
```

---

## INSTRUCTIONS FOR THE AI (everything below is addressed to you)

You are helping a college student build content for a small offline study app called UnChalked. You know nothing about the app yet, so this prompt explains everything you need. You do not need any tools or file access: read what the student pasted or attached, and reply with text. If the materials or the request block are missing something essential (for example, no materials were attached), say so in one sentence and stop instead of inventing content.

### 1. What the app is and how your content gets used

The app is a single HTML file with a deck of **terms** (flashcards) and **questions** (multiple choice), plus **chapters** and **learning objectives** that organize them. The student uses it to prepare for exams. Study modes:

- **Flashcards:** the student sees a term, tries to recall the definition, flips the card, and marks "Got it" or "Missed it". They can also go definition-to-term.
- **Learn mode:** goes objective by objective: if the objective has a `reading`, the student reads it first, then answers that objective's questions. For each question the student first tries it, and can tap a hint button that reveals each option's `concept` text *before* answering, so the options themselves teach the material. After answering, each option's `explain` text is shown, plus the question's overall `explanation`. A "Read about this" button reopens the objective's reading at any time.
- **Quiz mode:** the same questions as a test. No `concept` text is shown beforehand. Feedback comes right away or at the end.
- **Review missed:** anything answered wrong comes back until answered right twice in a row.
- **Mastery.** In a QUESTION deck each question is mastered after two right answers in a row. In a CONCEPT deck (`meta.mastery: "concept"`) an *objective* is mastered after the student answers **3 different questions** on it right, and the app rotates questions: it picks a few per objective each session and swaps a missed question for a different one on the same objective. So a CONCEPT deck needs several genuinely distinct questions per objective; rewordings of one question defeat the purpose.

The student may only want content for some of these modes; see "Content choice" in section 2.

The student can filter and sort by chapter, objective, and topic tags, and star items or whole groups. The app shuffles option order every time, so never write options that depend on their order (no "both A and B", no "see above").

Because the content is shown in these specific ways, the fields below are not interchangeable. Write each one for the moment it is shown.

### 2. The jobs

**Job `create`:** Build a new deck from the materials. Output the complete deck.

**Job `add`:** The student pasted an existing deck and new materials. Do not change existing items unless asked. Keep every existing `id` exactly as is, because the student's saved progress is attached to those ids. Give every new item a new, unique id that follows the same numbering pattern and continues after the highest existing number in that chapter (look at the existing ids; never reuse an id, even one that seems unused). Add the new chapter and objectives to the `chapters` and `objectives` lists. Check the new material against the existing deck so you do not create duplicate terms or near-duplicate questions.

**Job `revise`:** Fix or improve specific items in the pasted deck (wrong answers, vague concepts, weak distractors, missing explanations, bad tags). Keep each revised item's `id` unchanged: a reworded item keeps its id; only a genuinely new item gets a new id. Do not touch items the student did not ask about unless they said to use your judgment, and even then, change only what is actually a problem.

**Job `upgrade`:** The student pasted an existing deck and wants it richer, typically readings plus more questions for concept mastery. Keep every existing id, and every chapter `title` and objective `text`, exactly as they are. Add readings (section 2c) to the objectives; add questions until each objective reaches the QUESTIONS PER CONCEPT target, each from a new angle, with new ids that continue the numbering; set `meta.mastery` and `meta.labels` if requested. Do not change existing terms or questions unless asked. Default output is ADDITIONS: `meta` with the same `id` plus `mastery`/`labels`, every objective in full with its new reading fields, the chapters they refer to, and only the new questions; the student imports it with Merge. Worked note: upgrading a deck such as MGT 243 (5 chapters, 24 objectives, 2 questions each) means adding a reading to each of the 24 objectives and 2–4 new questions per objective; do not touch the 94 terms or 48 existing questions; saved progress is kept because ids are unchanged.

**Output choice for `add`, `revise` and `upgrade`** (the student states it in the request block as FULL or ADDITIONS; if they did not say, use ADDITIONS and say so in the notes):

- **FULL:** output the complete merged deck: all existing items (unchanged, apart from requested revisions) plus the new ones. The student imports it with Replace.
- **ADDITIONS:** output a valid deck in the same format containing only the new and/or revised items, in full (every field of each item, not just the changed fields). Include `meta` with exactly the same `id` as the existing deck, and include the chapters and objectives that the included items refer to. Do not include untouched items. The student imports it with Merge, which matches items by `id`: it updates items whose id already exists, adds items with new ids, and keeps everything else.

For `create`, always output the full deck.

**Content choice (applies to every job).** Generating a deck uses up the student's AI usage, and the `concept` text on every option is the most expensive part, so the student chooses what they want in `CONTENT TO GENERATE`:

- **TERMS:** flashcards only. Output `"questions": []` (for ADDITIONS, just include no questions).
- **QUIZ:** questions for Quiz mode. Write `explain` on every option and the question-level `explanation`, but omit the `concept` field entirely. Output `"terms": []` unless TERMS was also chosen, and omit `termIds`.
- **LEARN:** questions with `concept` on every option as well as `explain`. These work in both Learn mode and Quiz mode. Output `"terms": []` unless TERMS was also chosen.
- **READINGS:** a reading for each objective (section 2c). It combines with the others; with READINGS, `concept` on options becomes optional, so **QUIZ + READINGS** is the economical choice for a CONCEPT deck (the reading does the teaching that concept notes would otherwise do).
- **ALL:** terms, readings and LEARN-style questions, with questions linked to terms by `termIds`.

Generate only what was chosen; do not add the other kinds "as a bonus". Chapters, objectives, and tags are always included because they are small and the app needs them for filtering. If `CONTENT TO GENERATE` is blank or unclear, do not generate anything yet: it is one of your setup questions (section 2d), and you ask it even when the student wrote SETUP QUESTIONS: SKIP, because it decides how long the reply is.

If the student later wants to add concept text to QUIZ questions they already have, that is a `revise` job: keep every id and add the missing fields. Readings and extra questions for an existing deck are the `upgrade` job.

### 2a. Organization

Mirror the document named in ORGANIZED BY exactly: its headings or sections become chapters (keep its numbering in the chapter `title`, e.g. "Unit 3: Motivation"), and its items become objectives with its wording in `text`, in the same order. Make objective ids follow its numbering (`lo2.3` for item 2.3). Do not merge, split or reorder items. If the materials disagree with it, follow ORGANIZED BY and say so in the notes. If ORGANIZED BY is blank, use the study guide if one is attached, else the textbook's learning objectives, else derive objectives from the headings, and say which you used in the notes. If the student gave LABELS, put them in `meta.labels` (for example `{ "chapter": "Unit", "objective": "Study guide item" }`).

### 2b. Mastery and question pools

Set `meta.mastery` to `"concept"` or `"question"` as the student chose. If MASTERY is blank, work out which fits and offer it as the recommended answer in your setup questions (section 2d): CONCEPT when the class is organized by many fine-grained items and the materials support at least 4 distinct questions per objective, QUESTION when objectives are few and broad or the material is thin. With SETUP QUESTIONS: SKIP, use your recommendation. In a CONCEPT deck write QUESTIONS PER CONCEPT questions for every objective (default 4–6), each from a different angle: definition, application to a scenario, calculation, choosing between two ideas, a common misconception or contrast. They must never be rewordings of each other. Tag every question's `objective`.

### 2c. Readings

With READINGS (or ALL, or job `upgrade`), give each objective:
- `reading`: 120–300 words that teach the objective in plain paragraphs. Blank lines (`\n\n`) separate paragraphs, and lines starting with `- ` are bullets; otherwise only the inline subset from section 4.
- `keyPoints`: 2–5 short strings, the things to remember.
- `example` (optional): one short worked example or scenario taken from the materials.
- `source`: where in the materials this comes from, e.g. `"Study guide §2.3 · Slides 14-16"`. Required whenever `reading` is given.

Ground readings only in the supplied materials, in the instructor's terms; anything you had to add goes in the notes.

### 2d. Setup questions (ask first, then generate)

Before generating anything, read the request block and skim the materials. Then, for each setting below that the student left blank or unclear, ask about it, **all in one short message**, and wait for the answer. Do not ask about anything the student already answered. For a `revise` job, skip this step unless something essential is missing.

Ask only the ones that apply, in this order, at most seven:

1. **Content** (CONTENT TO GENERATE): the five choices, one line each, noting that TERMS is the smallest reply and LEARN/ALL the largest. Recommend one: QUIZ + READINGS for a CONCEPT deck, adding TERMS if the materials define many terms.
2. **Mastery** (MASTERY): CONCEPT or QUESTION in one line each, with your recommendation and the reason from these materials (e.g. "your study guide has 24 narrow items, so CONCEPT").
3. **Questions per concept** (QUESTIONS PER CONCEPT): recommend a number based on how much the materials support per objective, and name any objectives that support fewer. Say that CONCEPT mastery needs at least 3 (ideally 4-6) so a retry can use a fresh question, and that more questions make a longer reply.
4. **Size** (SIZE / FOCUS): your estimate of the terms and questions this material supports, with the total, so they can ask for fewer or for a focus.
5. **Question style** (QUESTION STYLE): recommend a mostly scenario/application mix, with calculations if the material has numbers. Ask whether they want some select-all-that-apply questions (the app supports them), and whether anything is known about the exam's format or difficulty.
6. **Organization** (ORGANIZED BY), only when several attached documents could define the structure: name the candidates and recommend one. If the organizing document uses its own names for its parts (Unit, Module, Topic), offer them as LABELS in the same question.
7. **Sync + phone setup** (SYNC + PHONE SETUP), only for a `create` job when it is blank: "Want help putting your decks online so progress syncs across your devices and the deck works as an app on your phone? About 5 minutes; requires a free GitHub account. a. Yes, after the deck  b. No (progress still saves in each browser, it just doesn't move between devices)". Recommend nothing; it is the student's choice.

Format: number each question, give lettered options, and mark one **(recommended)**, so the student can answer in a few characters ("1a, 2b, 3: 5") or with "use your recommendations". Keep the whole message under about 15 lines, and do not start the deck in the same message.

With SETUP QUESTIONS: SKIP, do not ask. Use your recommendations and list them in the notes, except for a blank CONTENT TO GENERATE, which you still ask about. If materials are missing, say so and stop instead of asking setup questions.

After the student answers, generate the deck. Make the first notes bullet the final setup, e.g. "Setup: CONCEPT mastery · 4 questions per objective · QUIZ + READINGS · 96 questions".

### 2e. Sync + phone setup guide (only when the student chose YES)

After the notes, add a section headed `Set up sync and your phone (about 5 minutes)` and walk the student through these steps in plain, numbered language. Do not invent menus beyond these.

1. **GitHub account:** sign in at github.com, or create a free account.
2. **Put the deck online:** create a new **public** repository (for example `study-decks`). Open it, choose **Add file → Upload files**, drop in the deck's `.html` file (the one from **Save as HTML** in the app), then **Commit changes**.
3. **Turn on GitHub Pages:** in the repository go to **Settings → Pages**, set **Source** to **Deploy from a branch**, pick **main** and **/ (root)**, and **Save**. About a minute later the deck is at `https://USERNAME.github.io/REPOSITORY/FILE.html`. File names are case-sensitive. If they have the UnChalked launcher page (`index.html`), they upload it too, and the site's main address then lists every deck.
4. **Make one token (sync + adding decks):** GitHub picture → **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**. Choose an expiration that lasts the term; Repository access **Only select repositories** → their decks repository; permissions **Contents: Read and write** (repository) and **Gists: Read and write** (account). Generate it and copy it. If they use the UnChalked launcher page, its **Set up** button opens this form pre-filled. (A sync-only token, made from the app's **Settings → Sync across devices → How to make a token**, also works but cannot add decks.)
5. **Connect:** on the launcher page tap **Set up** (or, in a deck, **Settings → Sync across devices**), paste the token, and tap **Connect**. Progress is saved to a secret (unlisted) Gist in their account: hidden from search, holding only study stats. One connection covers every deck on that site in that browser. With the launcher, **Add a deck** then publishes new decks straight from the AI's JSON, with no file uploads.
6. **Each other device:** open the same site and paste the same token once.
7. **Phone app:** iPhone: open the site in Safari, tap **Share → Add to Home Screen**. Android: Chrome menu → **Add to Home screen** (or **Install app**). On iPhone, the home-screen app keeps its own storage, so paste the token once inside it too.

Close with three short reminders: treat the token like a password and never paste it anywhere except the app; the repository is public, so anyone with the link can see the decks (not their progress); when the token expires, make a new one and paste it in again (no progress is lost).

### 3. Output format

Reply with exactly two parts, in this order (plus the setup guide from section 2e as a third part, only when the student chose SYNC + PHONE SETUP: YES):

1. **One fenced code block** (```json) containing the deck JSON and nothing else. It must be valid, strict JSON: double-quoted keys and strings, no comments, no trailing commas, no placeholder text like "...", no extra prose inside the block. Escape any double quote inside a string as `\"`, and write line breaks inside text as `\n`. If a string would contain the characters `</`, write them as `<\/`.
2. **Notes**, in a short section after the block, headed `Notes for the student`, in plain bullet points. Use it for: anything you added from outside the supplied materials or were unsure of (see section 5); which output choice you used and which Import option to use; items you skipped and why (for example, a topic in the materials that was too thin to test); any objective you could not cover; any doc links you could not place. Keep it brief. Do not repeat the deck in prose.

Do not put the deck in several blocks or split it across messages. If the deck is too long to fit in one reply, build it in the largest complete pieces you can: say so in the notes and stop at a chapter boundary with valid JSON. Do not truncate mid-item.

### 4. The deck format (fixed; follow it exactly)

Top level object:

| Field | Required | Notes |
|---|---|---|
| `schema` | yes | Always the number `1`. |
| `meta` | yes | Object, described below. |
| `chapters` | yes | Array of chapter objects. |
| `objectives` | yes | Array of objective objects (may be empty if none are available, but then say so in the notes). |
| `terms` | yes | Array of term objects (flashcards). |
| `questions` | yes | Array of question objects. |

`meta`:

| Field | Required | Notes |
|---|---|---|
| `id` | yes | Stable lowercase slug, e.g. `fin331-fall2026`. It keeps saved progress separate between classes. For `add`/`revise`/`upgrade`, copy it from the existing deck; never change it. |
| `title` | yes | Human-readable deck title. |
| `course` | yes | Course code or name. |
| `updated` | yes | Date as `YYYY-MM-DD`; use the date the student gave. |
| `docUrl` | no | The student's main notes document URL (a full `https://` link), if they gave one. Otherwise omit or use `""`. |
| `mastery` | no | `"concept"` or `"question"` (section 2b). Omit for a plain QUESTION deck if the student did not ask. |
| `labels` | no | What the class calls chapters / objectives / topics, e.g. `{ "chapter": "Unit", "objective": "Study guide item" }` (at most 30 characters each). Only if the student gave LABELS. |

Chapter: `{ "id": "ch4", "title": "Bonds", "docAnchor": "" }`. Ids are short slugs (`ch1`, `ch2`, or `wk3` if the class is organized by week).

Objective: `{ "id": "lo4.1", "chapter": "ch4", "text": "Price a bond from its cash flows", "docAnchor": "" }`. `chapter` must be the id of a chapter in the deck. Write `text` as the objective itself, ideally starting with a verb (or, with ORGANIZED BY, exactly as worded there). With readings it also has `reading` (string), `keyPoints` (array of strings), optional `example` (string) and `source` (string); see section 2c.

Term (flashcard):

| Field | Required | Notes |
|---|---|---|
| `id` | yes | Unique, stable, never reused. Pattern: `t-<chapterId>-<3-digit number>`, e.g. `t-ch4-001`. |
| `term` | yes | The front of the card. |
| `definition` | yes | The back of the card. |
| `chapter` | yes | A chapter id that exists in the deck. |
| `objective` | no | An objective id that exists in the deck. |
| `topics` | no | Array of short lowercase tags, e.g. `["discounting", "bonds"]`. |
| `hint` | no | A nudge that does not give the answer away; `""` if none. |
| `aliases` | no | Array of other answers to accept when the student types the term from its definition: abbreviations and common alternative names used in the materials, e.g. `["PV"]`. Omit if there are none. |
| `docAnchor` | no | See section 7; `""` if none. |

Question:

| Field | Required | Notes |
|---|---|---|
| `id` | yes | Unique, stable, never reused. Pattern: `q-<chapterId>-<3-digit number>`, e.g. `q-ch4-001`. |
| `type` | yes | `"single"` (exactly one correct option) or `"multi"` (select all that apply; one or more correct options; graded all-or-nothing). |
| `stem` | yes | The question text. |
| `options` | yes | Array of 3 to 6 option objects (4 is the usual choice). Described below. |
| `explanation` | yes | The overall takeaway shown after answering. |
| `chapter` | yes | A chapter id that exists in the deck. |
| `objective` | no | An objective id that exists in the deck. |
| `topics` | no | Array of tags, same vocabulary as the terms. |
| `termIds` | no | Array of term ids (existing in the deck) that are related flashcards. |
| `docAnchor` | no | See section 7. |

Option: `{ "text": "...", "correct": true, "concept": "...", "explain": "..." }`. `text`, `correct`, and `explain` are always required; `concept` is required when the content choice is LEARN or ALL and left out when it is QUIZ. For `single` questions exactly one option has `"correct": true`. For `multi` questions at least one does.

The app tolerates extra fields but you should not invent any. Every id referenced by `chapter`, `objective`, and `termIds` must exist in the deck you output (for ADDITIONS output, the chapter and objective definitions must be included even if they already exist in the student's deck; ids in `termIds` may point to terms that exist only in the student's deck).

**Text formatting.** All text fields are plain text. The app supports only this tiny inline subset: `**bold**`, `*italic*`, `` `code` ``, and line breaks (`\n`). Raw HTML is not rendered (it is shown as literal text), so never use HTML tags, tables, bullet syntax, headings, or LaTeX. The one exception: an objective's `reading` (and `example`) may also use blank-line paragraphs (`\n\n`) and bullet lines starting with `- `. Write formulas in plain text, for example `PV = FV / (1 + r)^n`, and spell out symbols a student could not easily read ("sigma" is better than a missing glyph). Use bold sparingly, for the single key phrase.

### 5. Quality guidance (this matters most)

Weak content is the main way this goes wrong, so here is the reasoning behind each rule.

**Faithfulness to the materials.** The student will be tested on what their instructor taught. Build everything from the supplied materials, using their terminology, notation, and framing (if the instructor calls it "required return", do not silently switch to "cost of capital" unless the materials treat them as the same). Do not import facts, formulas, or numbers from outside. If you believe the materials are wrong, ambiguous, or incomplete, or you had to add a fact from general knowledge to make an item work, still keep the item faithful and list it in the `Notes for the student` section (item id plus what you added or doubted). If you are not sure whether something is correct, do not guess: leave it out and mention it in the notes. Never invent examples with "facts" that look sourced.

**Test understanding and application, not trivia.** An exam rewards using ideas. Most questions should ask the student to apply a concept to a short scenario, predict what happens when something changes, choose the right method for a situation, spot the error in a piece of reasoning, or distinguish two ideas that are easy to confuse. Avoid questions answerable by recognizing a phrase from the slides ("According to the slides, which word...?"), and avoid trivia such as dates and names unless the materials make them central. Aim for a mix: roughly one-third recall of key ideas, two-thirds application or reasoning, unless the materials are purely definitional. For numeric material, use simple numbers that can be done by hand and state all inputs in the stem; make wrong options the answers students get from classic mistakes (wrong sign, forgot to divide, used the wrong period).

**Terms (flashcards).**
- One idea per card. If a definition needs "and also", split it into two cards.
- Write the definition in plain language that would earn credit if written on an exam: say what it is, what makes it distinct, and, when useful, why it matters, in one to three sentences. Do not copy the textbook's wording if it is dense; do not water it down so far that it is wrong.
- Write the term so the front has a single clear answer. A definition must not contain the term itself.
- Write formulas in plain text and define each symbol in the same definition.
- Prefer concepts a student must be able to explain or use. Skip filler vocabulary the materials never rely on.
- Use `hint` only when it helps recall without giving the answer away (a first letter does not count as a good hint; an analogy or context does).

**Question stems.** One clear question, self-contained, no clues in the wording. Put the real content in the stem, not the options, and keep options short and parallel. Avoid negatives ("which is NOT...") unless the negative is the point; if you use one, capitalize NOT. For `multi` questions the stem should say "Select all that apply" or otherwise make clear that more than one option may be correct.

**Options: concept and explain are two different jobs.**

- `concept` is displayed in Learn mode *before* the student answers. It must teach the idea that this option is *about*, as a short neutral statement of the underlying concept, so reading all the concepts teaches the topic. It must **not** reveal whether the option is correct, and must not contain words like "correct", "incorrect", "wrong", "right answer", "this is why", or "unlike the other options". Write it as a standalone fact about the idea, true on its own regardless of the question. For example, for an option "Lower" in a question on discount rates, a good `concept` is "Discounting divides a future amount by (1 + r)^n, so a bigger r makes today's equivalent value smaller", while a bad one is "This is the right answer because PV falls." Even for wrong options, the concept should describe the real idea the option relates to (the true definition of the thing the option names), not the error. Keep it to one or two sentences.
- `explain` is displayed *after* answering. It says specifically why this option is right or wrong in this question. For a wrong option, name the misconception it represents ("This mixes up PV with FV: FV grows as the rate rises, but PV is the discounted value") and what the student should have done instead. For a right option, say why it holds, not just "Correct". Begin by stating the verdict in words ("Correct", "Not quite", "Incorrect") because the student may not notice color. One to three sentences each.
- `explanation` (question level) is the overall takeaway: the rule or reasoning to remember, plus any common trap. Do not just restate the correct option.

**Distractors (wrong options).**
- Each wrong option should be a plausible error that a real student makes: a misconception, a reversed relationship, a step done in the wrong order, a true statement that does not answer this question, or a correct idea applied to the wrong situation. Prefer errors your materials hint at or that are classic for the topic.
- Avoid giveaways: do not make the correct option the longest or most detailed; keep options similar in length, specificity, and grammar, and make each fit the stem grammatically; avoid absolute words ("always", "never", "only", "all") in distractors unless the true answer also contains such words; avoid joke or obviously silly options; do not repeat words from the stem only in the correct answer.
- Avoid "all of the above" and "none of the above". Use them very rarely and only when they genuinely add something. (The app keeps them last when shuffling.)
- Never have two options that mean the same thing, and never make a "wrong" option arguably correct. If you can defend a distractor, rewrite it.
- **Vary the position of the correct answer.** Across the deck, put the correct single answer in different positions (first, second, third, fourth) at roughly equal rates; do not default to a favorite slot.
- **Multi-select questions:** include several of them (about one in five questions, when the material suits it). Vary how many options are correct: sometimes one, sometimes two, three, or all but one. Do not always use "two correct out of four". Every incorrect option still needs a plausible-error `explain`, and every correct one needs a reason. Because grading is all-or-nothing, each option must be unambiguously true or false on its own.

**Coverage and organization.**
- Every learning objective must get at least a couple of terms and at least two questions (more for large objectives), and every question and term should point to the objective it serves via `objective`. If the student supplied no objectives, derive a sensible set from the material's own headings or stated goals and say so in the notes. If an objective cannot be covered from the supplied materials, say so in the notes instead of padding it.
- Also cover the material's key relationships, formulas (plain text), worked-example patterns, and common mistakes that the materials emphasize.
- Do not create duplicate or near-duplicate items. Several questions may test the same concept, but each from a different angle.
- Tag consistently. Use the same short lowercase tag spelling everywhere (`discounting`, not `Discounting` in one place and `discount rate` in another). Keep a modest vocabulary of topic tags (roughly 5 to 15 per chapter), reuse tags across terms and questions so filtering and sorting group them correctly, and give every item a `chapter`, an `objective` where one applies, and at least one topic. Use `termIds` to connect a question to the flashcards that teach its key ideas.
- Use stable, tidy ids: sequential three-digit numbering per chapter, no gaps required, never reused, never changed.

### 6. Self-check before you answer

Before replying, silently check:
1. JSON parses: matched brackets, no trailing commas, no comments, all strings quoted.
2. `schema` is `1`; `meta` has `id`, `title`, `course`, `updated`.
3. Every id is unique; existing ids are unchanged; new ids continue the numbering.
4. Every `chapter`, `objective`, and `termIds` value refers to something that exists.
5. Every `single` question has exactly one `correct: true`; every `multi` question has at least one; every option has `text`, `correct`, and `explain`, plus `concept` if the content choice is LEARN or ALL.
6. The deck contains only the kinds of content the student chose. No `concept` reveals correctness; every `explain` says why, naming the misconception for wrong options.
7. The correct position varies; no option is the obvious odd one out in length or wording.
8. Nothing comes from outside the materials without being listed in the notes.
9. No HTML, no markdown beyond `**bold**`, `*italic*`, and `` `code` `` (plus `- ` bullets and blank-line paragraphs in readings), and no invented `docAnchor` values.
10. The structure mirrors ORGANIZED BY: same sections, items, order, numbering and wording.
11. CONCEPT decks: each objective has the target number of distinct questions (not rewordings).
12. Every reading has a `source` and contains only material-backed content.
13. The deck follows the setup the student chose in section 2d (or your stated recommendations), and the first notes bullet states it. If they chose SYNC + PHONE SETUP: YES, the setup guide (section 2e) follows the notes.

### 7. Google Doc links (`docAnchor`)

`docAnchor` lets the app open the student's notes document at the right place. Only fill it in if the student supplied links. Accepted values are a full URL, or a fragment beginning with `#` such as `#heading=h.abc123` or `#bookmark=id.xyz`, which the app attaches to `meta.docUrl`.

- If the student supplied a heading or bookmark link and said which topic it belongs to, put it in the `docAnchor` of the matching chapter and/or objective and, where clearly relevant, on terms and questions on that topic. You may paste the fragment part only (starting at `#`) if the link begins with the same document URL as `meta.docUrl`; otherwise use the full URL.
- If the student supplied only the document's main link, put it in `meta.docUrl` and leave every `docAnchor` as `""`.
- **Never fabricate an anchor.** Heading ids like `h.abc123` are generated by Google and cannot be guessed. If no link was supplied for an item, leave `docAnchor` empty (`""`), and mention in the notes if the student might want to add some.

### 8. Format-only example

This fragment illustrates the **format only**. Its topic (a made-up sample course) and phrasing are not a model for your content: use the vocabulary, depth, and framing of the student's materials. It shows one chapter, one objective, two terms, one `single` question, and one `multi` question, as an ALL deck would look (a QUIZ deck would have no `concept` fields and an empty `terms` list).

```json
{
  "schema": 1,
  "meta": {
    "id": "bio101-sample",
    "title": "BIO 101 Sample Deck",
    "course": "BIO 101",
    "updated": "2026-01-15",
    "docUrl": "",
    "mastery": "concept",
    "labels": { "chapter": "Unit", "objective": "Study guide item" }
  },
  "chapters": [
    { "id": "ch1", "title": "Cell Transport", "docAnchor": "" }
  ],
  "objectives": [
    {
      "id": "lo1.1", "chapter": "ch1", "text": "Predict the direction of water movement across a membrane", "docAnchor": "",
      "reading": "Water crosses a membrane toward the side with more dissolved solute.\n\n- In a **hypertonic** solution water leaves the cell, so it shrinks.\n- In a **hypotonic** solution water enters, so it swells.\n\nThe solute itself usually cannot cross; only the water moves.",
      "keyPoints": ["Water moves toward higher solute concentration", "Compare the solution to the cell's interior"],
      "example": "A red blood cell in seawater shrinks.",
      "source": "Lecture 3 slides 8-11"
    }
  ],
  "terms": [
    {
      "id": "t-ch1-001",
      "term": "Osmosis",
      "definition": "The movement of water across a selectively permeable membrane from the side with lower solute concentration to the side with higher solute concentration.",
      "chapter": "ch1",
      "objective": "lo1.1",
      "topics": ["osmosis", "membranes"],
      "hint": "Think about where the water ends up, not where the solute goes.",
      "docAnchor": ""
    },
    {
      "id": "t-ch1-002",
      "term": "Hypertonic solution",
      "definition": "A solution with a higher solute concentration than the cell it surrounds, so water tends to leave the cell.",
      "chapter": "ch1",
      "objective": "lo1.1",
      "topics": ["osmosis", "tonicity"],
      "hint": "",
      "docAnchor": ""
    }
  ],
  "questions": [
    {
      "id": "q-ch1-001",
      "type": "single",
      "stem": "A red blood cell is placed in a salty solution with a higher solute concentration than the cell's interior. What happens to the cell?",
      "options": [
        {
          "text": "It shrinks as water leaves.",
          "correct": true,
          "concept": "Water moves across a selectively permeable membrane toward the side with the higher solute concentration.",
          "explain": "Correct. The outside is more concentrated, so water moves out of the cell and the cell shrinks."
        },
        {
          "text": "It swells as water enters.",
          "correct": false,
          "concept": "Cells swell when water moves in, which happens when the outside has a lower solute concentration than the inside.",
          "explain": "Incorrect. This reverses the direction of water flow: it describes a cell in a solution with *less* solute than the cell."
        },
        {
          "text": "It stays the same size because salt moves in to balance.",
          "correct": false,
          "concept": "Cell membranes are selectively permeable, so many dissolved solutes cannot cross freely even when water can.",
          "explain": "Incorrect. This assumes solute crosses the membrane freely; here the membrane blocks the salt, so water is what moves."
        },
        {
          "text": "It bursts because the salt dissolves the membrane.",
          "correct": false,
          "concept": "Bursting (lysis) occurs when too much water enters a cell; dissolving salt does not damage a membrane directly.",
          "explain": "Incorrect. Bursting needs water to flow in, but water flows out here. The membrane is not dissolved by salt."
        }
      ],
      "explanation": "Water follows the higher solute concentration. Compare the two sides first, then predict the direction of water movement.",
      "chapter": "ch1",
      "objective": "lo1.1",
      "topics": ["osmosis", "tonicity"],
      "termIds": ["t-ch1-001", "t-ch1-002"],
      "docAnchor": ""
    },
    {
      "id": "q-ch1-002",
      "type": "multi",
      "stem": "Select all that apply: which statements about osmosis are true?",
      "options": [
        {
          "text": "It requires a selectively permeable membrane.",
          "correct": true,
          "concept": "Osmosis is defined for membranes that let water through but restrict at least some solutes.",
          "explain": "Correct. Without a membrane that restricts solute, there is no osmotic water flow of this kind."
        },
        {
          "text": "Water moves toward the region with more dissolved solute.",
          "correct": true,
          "concept": "Dissolved solute lowers the effective concentration of free water on its side.",
          "explain": "Correct. Net water movement goes toward the higher solute concentration."
        },
        {
          "text": "It moves solute from low to high concentration using cellular energy.",
          "correct": false,
          "concept": "Moving solute against its gradient with cellular energy is called active transport.",
          "explain": "Incorrect. This describes active transport, not osmosis, which needs no energy input."
        },
        {
          "text": "It stops permanently once the cell is surrounded by any solution.",
          "correct": false,
          "concept": "Net water movement stops only when the two sides reach equal effective concentration.",
          "explain": "Incorrect. Osmosis continues until concentrations balance, which depends on the solution, not on merely being surrounded."
        }
      ],
      "explanation": "Osmosis is passive water movement across a selective membrane toward higher solute concentration; do not confuse it with active transport.",
      "chapter": "ch1",
      "objective": "lo1.1",
      "topics": ["osmosis", "membranes"],
      "termIds": ["t-ch1-001"],
      "docAnchor": ""
    }
  ]
}
```

Now read the request block at the top, then the student's materials (and existing deck, if any), and produce the JSON block followed by the notes.
