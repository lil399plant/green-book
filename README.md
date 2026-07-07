# QuantPrep — the green book, drilled

An interactive study app for quant-trading interview prep, built around the problem set in
Xinfeng Zhou's *A Practical Guide to Quantitative Finance Interviews*.

It's a **single HTML file** — no build step, no dependencies. Open it and go.

## Features

- **Learn** — browse problems by module, reveal hint → solution, mark mastered.
  - **Coach**: type your own reasoning and an AI checks it, nudging you with escalating hints instead of dumping the answer.
  - **→ Related problem**: jumps you to another problem in the book that trains the same technique.
  - **Market connection**: every problem shows where its technique shows up on a real trading desk (auto-revealed when you master it).
- **Flashcards** — Leitner spaced repetition (5 boxes); misses come back sooner, mastered cards fade out.
- **Quiz** — timed, scored sessions weighted toward your weak spots.
- **Tutor chat** — a floating assistant for any question, mid-session.
- **Progress persists** — mastery, boxes, quiz history, and streak are saved locally.

## Run it

**Option A — GitHub Pages**
1. Put `quantprep.html` in a repo (rename to `index.html` if you want it at the site root).
2. Settings → Pages → deploy from your branch.
3. Visit the URL.

**Option B — locally**
Just open `quantprep.html` in a browser, or serve the folder:
```bash
python3 -m http.server 8000   # then open http://localhost:8000/quantprep.html
```

## AI setup (coach + tutor)

All AI features route through **one adapter**, configured via the **⚙ button** in the top bar.

- **Inside a Claude Artifact**: it uses the built-in Claude runtime automatically — no key needed.
- **Anywhere else** (GitHub Pages, local file): open ⚙ Settings and pick a provider.

Supported providers (pick a **Quick preset** to auto-fill the endpoint + a default model):

| Preset      | Mode              | Base URL                          | Example model                 | Key |
|-------------|-------------------|-----------------------------------|-------------------------------|-----|
| DeepSeek    | openai-compatible | `https://api.deepseek.com`        | `deepseek-chat`               | yes |
| OpenAI      | openai-compatible | `https://api.openai.com/v1`       | `gpt-4o-mini`                 | yes |
| Groq        | openai-compatible | `https://api.groq.com/openai/v1`  | `llama-3.3-70b-versatile`     | yes |
| OpenRouter  | openai-compatible | `https://openrouter.ai/api/v1`    | `deepseek/deepseek-chat`      | yes |
| Ollama      | openai-compatible | `http://localhost:11434/v1`       | `llama3.1`                    | no  |
| Anthropic   | anthropic         | `https://api.anthropic.com`       | `claude-3-5-sonnet-latest`    | yes |
| Custom      | openai-compatible | your endpoint                     | your model                    | opt |

Any OpenAI-compatible `/chat/completions` endpoint works — set **Custom** and paste the base URL.
Use **Test connection** to confirm it before saving.

### Notes & gotchas
- **Keys are stored only in your browser** (`localStorage`) and are sent solely to the provider you configure. Nothing is proxied through the app. Because this is a static client-side app, a key you enter is visible to that browser — use your own key, and prefer a low-limit key for a public deployment.
- Some providers require **CORS / browser access** to be enabled for direct browser calls. OpenAI and DeepSeek generally allow it; if you hit a CORS error, run behind your own small proxy or use a provider that permits browser origins.
- The Anthropic-direct mode sends `anthropic-dangerous-direct-browser-access: true`, as required for calling the Anthropic API from a browser.

## Customizing the problem set

All problems live in the `P` array near the top of the `<script>` in `quantprep.html`:
```js
{ id, module, topic, title, q, hint, sol, ans, quiz:{stem, options, correct} }
```
`module`: `0` Brain Teasers · `1` Probability · `2` Stochastic · `3` Finance.
Market-connection blurbs live in the `MARKET` map (keyed by `id`). Add entries and they appear everywhere automatically.

## Credit
Problem content is adapted from Xinfeng Zhou's *A Practical Guide to Quantitative Finance Interviews*. This app is a study aid; buy the book for the full treatment.

# Getting started with QuantPrep

A walkthrough from the perspective of someone who just opened the app for the first time.

---

## 0. Open it
- **On your GitHub Pages URL**, or by opening `quantprep.html` locally.
- You land on the **Dashboard**: a hero header, four **module cards** (Brain Teasers, Probability, Stochastic, Finance) each showing `0%` mastery, an empty quiz log, and a `🔥 0-day streak` in the top-right.
- Nothing to install, no login. Progress starts saving the moment you interact.

---

## 1. Turn on the AI (one-time)
The **Coach** and **Tutor** need an AI provider. Look at the top bar:

- **Running inside a Claude Artifact?** Do nothing — it uses Claude automatically. The **⚙** badge reads `Claude`.
- **Self-hosted (GitHub Pages / local file)?** Click **⚙** and:
  1. Pick a **Quick preset** — e.g. *DeepSeek*, *OpenAI*, *Groq*, *OpenRouter*, *Ollama (local)*, or *Anthropic*. This fills in the endpoint + a default model.
  2. Paste your **API key** (skip for local Ollama).
  3. Click **Test connection** → you want `Connected ✓`.
  4. Click **Save**. The ⚙ badge now shows your model.

> Your key is stored only in your browser and is sent only to the provider you chose. If you skip this step, the study features still work — just the AI coach/tutor won't respond.

---

## 2. Learn a concept (the core loop)
Click **Learn** in the top nav.

1. **Filter** by module (or “All”). You'll see a list of problems with a topic tag and a status dot.
2. **Click a problem** to expand it. You get the full question.
3. **Try it yourself first.** In the **Coach** box, type your reasoning or answer and hit **Check** (or Ctrl/Cmd+Enter).
   - Right → it confirms and names the technique.
   - Partly right → it affirms what's correct and asks one pointed question.
   - Wrong → it gives the smallest nudge, no spoilers. Type “give me a hint” for more, or “I give up” for the full walkthrough.
4. Stuck without the coach? Use **Show hint**, then **Reveal solution**.
5. Click **Mark mastered** — the dot turns green, and the **Market connection** panel auto-opens, showing where that exact technique lives on a real trading desk.
6. Want another rep on the same idea? Click **→ Related problem** — it jumps you to a different green-book problem that trains the same technique and highlights it.

---

## 3. Reinforce with flashcards
Click **Flashcards**.

- A card shows a problem prompt. Click it (or press **Space**) to flip and see the answer + a hint.
- Grade yourself: **→ / Got it** promotes the card toward box 5; **← / Missed it** sends it back to box 1.
- The app uses **Leitner spaced repetition** — cards you miss reappear more often; mastered ones fade out. The box pips at the top show how your deck is distributed.

---

## 4. Test yourself
Click **Quiz**.

1. Choose a focus (Mixed, or a single module) and **Start quiz**.
2. 10 multiple-choice questions, **25 seconds each**, weighted toward your weak / unmastered problems.
3. Pick an answer → it marks correct/wrong and shows the “why”. Hit **Next**.
4. At the end you get a **score %** and it's logged on the Dashboard. Correct answers bump those problems up a box; misses drop them to box 1 so they resurface.

---

## 5. Ask anything, anytime
The **💬 bubble** (bottom-right) opens the **Tutor** from any tab.
- Ask a concept (“explain Itô's lemma”), request a hint, or ask how a technique maps to markets.
- It remembers the conversation. **Enter** sends, **Shift+Enter** = newline, **Esc** closes.

---

## 6. Come back tomorrow
- Everything you did — mastery, flashcard boxes, quiz history, streak — is **saved on this device**.
- Reopen the app and your Dashboard reflects where you left off; studying on consecutive days grows your streak.

---

## A good daily rhythm
1. **Dashboard**: glance at your weakest module.
2. **Learn**: work 3–5 problems in that module using the Coach; mark them mastered and read the market connection.
3. **Flashcards**: a quick pass to keep older material warm.
4. **Quiz**: one 10-question round to pressure-test recall.
5. Use the **Tutor** whenever something doesn't click.

That's the whole loop: *attempt → get coached → see the market payoff → drill → test → repeat.*
