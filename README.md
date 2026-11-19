# UnChalked

Turn your class materials into flashcards, quizzes and short readings, with an AI chat doing the writing. UnChalked runs in your browser and works offline. It syncs your progress across your devices and installs on your phone like an app.

**Try the demo:** https://unchalked.github.io/unchalked/

---

## Fastest start: let your AI walk you through it

Paste this into any AI chat (ChatGPT, Claude, Gemini…):

> Set me up with UnChalked: https://github.com/unchalked/unchalked

If your AI can't open links, paste this whole page into the chat instead.

Plan on about 10 minutes and a free GitHub account. Without a GitHub account you can still use the [offline version](#offline-version-no-github-account).

---

## Setup by hand (about 10 minutes)

1. **GitHub account.** Sign in at [github.com](https://github.com), or create a free account.
2. **Make your copy.** On this page, click the green **Use this template** button, then **Create a new repository**. Pick a name such as `unchalked`, keep it **Public**, and click **Create repository**.
3. **Turn on your website.** In your new repository, open **Settings → Pages**. Under *Build and deployment*, set **Source** to **Deploy from a branch**, choose **main** and **/ (root)**, then click **Save**. About a minute later your site is live at `https://YOUR-USERNAME.github.io/REPOSITORY-NAME/`.
4. **Open your site** and follow its **Get started** checklist:
   - **Connect your GitHub token.** Tap **Set up**. GitHub's token form opens with everything filled in. Under **Repository access**, choose **Only select repositories** and pick your repository. Then **Generate token**, copy it, and paste it into UnChalked. This one token turns on sync across devices and lets you add decks from your site.
   - **Make your first deck.** Tap **Copy AI deck prompt** and paste it into any AI chat together with your class materials (study guide, slides, notes, textbook chapter). The AI asks a few quick setup questions, then gives you the deck as a block of JSON. Tap **Add a deck**, paste it in, and tap **Publish**. It appears about a minute later.
   - **On your phone:** in Safari tap **Share → Add to Home Screen** (Android: Chrome menu **⋮ → Add to Home screen**). Open UnChalked from the home screen and paste your token there once too, because the home-screen app keeps its own storage.
5. **Optional:** remove the demo deck with **⋯ → Remove from site** on its card.

## What you can do from your site

- **Add a deck:** paste the AI's JSON, add a friend's deck from a link, or upload a saved `.html` file. Paste the prompt's *add* or *upgrade* results to merge them into an existing deck.
- **Share with friends:** every deck has a **Share with friends** button. Your friend adds it with **Add a deck → From a link** and gets their own copy, with their own progress. When you improve the deck, their card shows **Update available**.
- **Updates:** when a new version of UnChalked comes out, your site shows **UnChalked update available**. One tap updates the app in all your decks. Your decks and your progress don't change.
- **Rename or remove decks** with the **⋯** button on each card.
- **Send feedback** with the link at the bottom of your site, or in any deck's **Settings**.

## Good to know

- **Your site is public.** Free GitHub Pages sites can be opened by anyone who has the link. Decks built from an instructor's materials might not be meant for public sharing, so check your course's rules before posting them.
- **Your progress** syncs through a "secret" Gist in your GitHub account. Secret Gists are unlisted, not locked: they don't show up in search, but anyone given the exact link could open one. It holds only study stats (which items you got right or wrong, and when).
- **Mistakes can be undone.** Every add, update, rename or removal is saved in your repository's history on GitHub (the **Commits** list), so an earlier version of a deck can always be recovered.
- **Your token is like a password.** Paste it only into UnChalked. It stays in your browser and is sent only to GitHub. If it ever leaks, delete it on GitHub (**Settings → Developer settings → Personal access tokens**) and make a new one.
- **When the token expires** (after a year), make a new one and paste it in again. No progress is lost.
- **Progress** saves in each browser even without sync. Sync just keeps your devices in step.

## Offline version (no GitHub account)

1. Download the app: open [the demo](https://unchalked.github.io/unchalked/demo.html) and save the page (**File → Save Page As…**, or on a phone **Share → Save to Files**). You can also download `demo.html` from this repository with the **Download raw file** button.
2. Open the saved file in your browser.
3. Get deck JSON from any AI chat with [DECK_PROMPT.md](DECK_PROMPT.md) and your class materials.
4. In the app go to **Edit → Import / export**, paste the JSON, choose **Replace**, and apply it. Then **Edit → Deck & file → Save as HTML** gives you your own deck file.

Progress then stays in that one browser.

---

## For AI assistants helping someone set this up

You are guiding a student through setting up UnChalked. They do every click themselves, and you explain one step at a time, waiting for them to confirm each one. **Never ask for, accept or handle their GitHub token or password.** If they paste a token into the chat, tell them to delete it on GitHub and make a new one.

1. Ask whether they have a GitHub account. If not: github.com/signup (free).
2. Have them open https://github.com/unchalked/unchalked, click **Use this template → Create a new repository**, name it (e.g. `unchalked`), keep it **Public**, and click **Create repository**.
3. Have them open **Settings → Pages** in their new repository and set **Source: Deploy from a branch**, **Branch: main**, **/ (root)**, then **Save**. Their site will be at `https://THEIR-USERNAME.github.io/REPOSITORY-NAME/` after about a minute (refresh until it loads).
4. On their site, the **Get started** checklist leads the way. For **Set up**: the token form is pre-filled, and they only need to choose **Only select repositories →** their repository, then **Generate token**. They copy it and paste it into UnChalked, not into the chat.
5. **First deck:** read the deck prompt at https://raw.githubusercontent.com/unchalked/unchalked/main/DECK_PROMPT.md and follow it exactly. Ask its setup questions, ask the student for their class materials, then reply with the deck JSON block and the notes it specifies. Tell them to tap **Add a deck** on their site, paste the JSON, and tap **Publish**. If you can't open that link, ask them to tap **Copy AI deck prompt** on their site and paste it to you.
6. **Phone:** Safari **Share → Add to Home Screen** (Android: Chrome **⋮ → Add to Home screen**), then paste the token once inside the home-screen app.
7. If they only want a one-time offline file with no GitHub account, use the *Offline version* steps above instead.

---

Questions or ideas: use **Send feedback** in UnChalked.

Current version: launcher 1.2 · app 5.0
