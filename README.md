# HumanWrite AI

Turns generic, robotic or over-polished text into clear, natural writing while keeping the facts, citations and the writer's meaning intact.

HumanWrite is a writing-quality tool. It doesn't claim to bypass Turnitin or any other AI detector. It improves the writing itself.

## Running it

No install or build step is needed.

1. Open `index.html` in Chrome, Edge or Firefox. Double-clicking it works.
   - If your browser blocks scripts on `file://` pages, run the tiny local server instead:
     `powershell -ExecutionPolicy Bypass -File serve.ps1` and open http://localhost:8080
2. Go to **Settings** and paste your Anthropic API key (from console.anthropic.com).
3. Paste text into **Rewrite**, press **Analyze writing**, then **Humanize text**.

Analysis runs entirely in your browser. Only the rewrite step sends text to the Anthropic API, straight from your browser using your key.

## Features

| Area | What it does |
|---|---|
| **Writing analysis** | 14 local checks: generic phrasing, repetitive structure, similar sentence lengths, inflated vocabulary, connector overuse, repeated phrases, repeated ideas, predictable paragraphs, unsupported claims, missing examples, passive voice, over-polished wording, unnatural flow and missing reasoning. It gives a score, the issues it found and a highlighted view of the text. |
| **Humanization engine** | Claude (Opus 5 by default) rewrites with varied rhythm, plain vocabulary and real reasoning. It never inserts errors, swaps synonyms word by word or invents facts. |
| **Meaning preservation** | The prompt locks in facts, statistics, names, dates, citations and the conclusion. After each rewrite the app checks locally that every figure, date, citation and quotation still appears. |
| **Unsupported claims** | These are flagged with suggestions for what to add. The app doesn't fabricate evidence. |
| **Writing profiles** | Learns your style from your own samples: sentence length, vocabulary level, formality, point of view, sentence structures, explanation style, use of examples, paragraph length and recurring expressions. |
| **Modes** | Natural, Student, Professional, Conversational, Academic, Personal Voice |
| **Academic levels** | GCSE / O-Level, A-Level, UK Level 3, Undergraduate, Professional |
| **Controls** | Keep my wording / Lightly / Balanced / Rewrite more, plus sliders for simplicity, formality, sentence variation, vocabulary, personal voice, conciseness and explanation depth. You can also set spelling (UK/US) and turn on "explain changes". |
| **Before / after** | Side-by-side view coloured by change type (restructured, generic wording removed, repetition removed, explanation improved, vocabulary simplified, flow improved, voice adjusted). You can switch to a word-level diff or clean text. |
| **Why this sounds better** | Specific reasons for the changes, a before/after metrics table and a clickable change log. |
| **Refine** | Follow-up instructions such as "make paragraph 2 simpler", with one-click suggestions. Only that change is applied, and facts are still checked against the original. |
| **Try another version** | A different take on the same text with the same settings. |
| **History** | The last 25 rewrites are saved in this browser. You can reopen, export or delete them, or switch saving off. |
| **Export** | Plain text, a Word document (.doc) or a full HTML report (both versions, reasons, metrics, meaning check, flagged claims and the change log). |
| **AI-use statement** | Copies a short, honest disclosure you can adapt for coursework. |
| **Long-text guard** | Asks you to confirm before rewriting texts over 5,000 words. |

## Project layout

```
index.html        UI shell
css/styles.css    Styles (light/dark themes, responsive)
js/text.js        Sentence splitting, word utilities, safe storage
js/analyzer.js    Local writing analysis and the meaning check
js/diff.js        Word diff and change-type highlighting
js/profile.js     Writing-profile analysis and storage
js/claude.js      Prompt construction and the Claude API call (Anthropic TypeScript SDK via CDN)
js/app.js         UI controller
serve.ps1         Optional local static server
```

## Notes

- The API key is kept in session storage by default, or in local storage if you tick "Remember key". Because the browser calls the API directly, anyone using this browser profile can read the key. Use a key with a spending limit. If you deploy this publicly, move the API call to a small backend.
- The rewrite uses structured JSON output, adaptive thinking and a cached system prompt. On Opus 5 it also sets `fallbacks: "default"`, so a request declined by a safety classifier is retried on Anthropic's recommended fallback model instead of failing.
