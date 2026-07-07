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

