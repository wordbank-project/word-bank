# Word Bank

**Turn the books you read into vocabulary you keep.**

Word Bank is a reading companion. Track what you want to read, are reading, or have read — and every time a word stops you, save it with its definition, your own sentence, and your notes. Your books and words live **on your device**, work **offline**, and need **no account**.

[![Download for Android (beta)](https://img.shields.io/badge/Download-Android%20(beta)-208AEF?logo=android&logoColor=white)](https://github.com/wordbank-project/word-bank-app/releases/latest)

_Web — [app.wordbankapp.com](https://app.wordbankapp.com) · iOS — coming soon_

## ❤️ Support Word Bank

Word Bank is free, open source, and has no ads — and it stays that way. There's no company behind it, just one developer and a few servers.

**Every donation goes to infrastructure**: the DigitalOcean droplet running the dictionary and word-feed APIs, and the domain behind them — currently about $6/month and $10/year. The AI features run on free tiers for now; that's the part most likely to grow. The app is free during the beta and will never carry ads or trackers, so running costs are all there is to cover.

[GitHub Sponsors](https://github.com/sponsors/jensrot) · [Liberapay](https://liberapay.com/jensrot) · [Ko-fi](https://ko-fi.com/jensrot) · [Buy Me a Coffee](https://buymeacoffee.com/jensrot)

<!-- TODO: confirm the Liberapay / Ko-fi / Buy Me a Coffee handles after registering -->

## 📦 Source code

| Repository | What's inside |
| --- | --- |
| **[word-bank-app](https://github.com/wordbank-project/word-bank-app)** | The Expo / React Native app — Android and web today, iOS on the way |
| **[word-bank-site](https://github.com/wordbank-project/word-bank-site)** | The marketing / showcase site (Astro, in English, Dutch and French) |
| **[word-bank-server](https://github.com/wordbank-project/word-bank-server)** | The community word feed behind the site's live word wall, plus the AI sentence-analysis and suggestion endpoints |
| **[wiktapi.dev](https://github.com/jensrot/wiktapi.dev)** | Self-hosted multilingual dictionary API (Wiktionary/kaikki data) |

## What it does

- **Track every book you read** — search millions of titles via Open Library (or add a custom book with its own cover photo) and sort them into _Want to read_, _Currently reading_, or _Have read_.
- **A word bank for every book** — each book keeps its own vocabulary; see at a glance how many words it has taught you.
- **Instant, precise definitions** — every meaning fetched at once, with part of speech and IPA pronunciation; search and pick the one that fits, colour-coded by part of speech.
- **Make words stick** — save each word with the sentence you found it in and your own notes; write a review and notes per book.
- **All your words in one place** — the Words List gathers every word from every book; search, filter by part of speech, and sort A–Z, by book, or by most recently added.
- **Practice what you saved** — flip-through flashcards with per-word "still learning" / "knew it" counters, an optional daily reminder, and a stats screen for what you keep forgetting.
- **Read in your language** — **English, French and Dutch** are live today, served from our own dictionary instance. The underlying Wiktionary data covers 100+ languages, so a self-hosted instance can serve any of them. See [Want another language?](#want-another-language) below.
- **Analyze a sentence with AI** — paste a sentence you got stuck on and get it explained in plain language, powered by [Groq](https://console.groq.com/).
- **Private & offline** — no account, no cloud, no tracking; everything is stored on your device.
- **Dark mode** included.

## Want another language?

**[Open an issue](https://github.com/wordbank-project/word-bank/issues/new) and name the language.** If it has a Wiktionary edition, I'll build it and deploy it to the dictionary server — no app update needed, the new language simply starts working.

Three are live today: English, French and Dutch. Adding one isn't a code change; it's importing that edition's Wiktionary data into the dictionary database and shipping it to the server on DigitalOcean. Kaikki publishes around twenty editions, so most widely-spoken languages are available.

Worth knowing before you ask: an edition describes words *in* that language — the French edition defines words in French. So "French" means French definitions of French words, which is what you want for reading in French, and not a French-to-English translation dictionary.

## Get started

1. **Download** the Android beta from [Releases](https://github.com/wordbank-project/word-bank-app/releases/latest), or skip the install and open [app.wordbankapp.com](https://app.wordbankapp.com).
2. **Find a book** — search by title or author, or add a custom book, and tap a reading status.
3. **Add words as you read** — type a word, pick the definition that fits, and anchor it with your own sentence and notes.

That's it.

> **Tip — an empty field is a suggestion.** Wherever text types itself into an input, pressing the button with the field still empty accepts whatever is showing. It works on book search, the custom-book title (which fills in the author and year to match), the add-a-word field, and the Analyze screen — all AI-generated examples in your chosen language. On the Words List it cycles words you have already saved instead. Handy when you want to try something without having to think of it first.

## Privacy

- **On-device** — your reading list, words, sentences, and notes are stored locally and work fully offline.
- **No account, no cloud, no tracking** — there is no server holding your data.
- **The community feed is anonymous** — [word-bank-server](https://github.com/wordbank-project/word-bank-server) stores only a saved *word* and its public dictionary data (definition, part of speech, phonetic) to power the marketing site's live word wall. It never receives your reading list, your sentences, or your notes.
- **One feature sends text you wrote** — "Analyze a sentence" posts that one sentence to the AI endpoint, per request, and says so on screen. Nothing else does.

## Architecture

```
                        ┌──────────────────────────────┐
                        │        word-bank-app         │   Expo / React Native
                        │   Android · web · iOS soon   │   — on-device, offline
                        └──┬─────────────┬───────────┬─┘
        book search        │  definitions│           │  saved words · AI
    ┌──────────────────────┘             │           └──────────────────┐
    ▼                                    ▼                              ▼
Open Library                       wiktapi.dev                  word-bank-server
(book catalog)                (en · nl · fr editions)      (word feed · Groq AI, Cerebras AI · SQLite)
                                                                        │
                                                                        ▼
                                                                 word-bank-site
                                                        (marketing · live word wall)
```

The app talks directly to the dictionary and book-search services and stores everything locally. The only data that leaves your device is anonymized saved words flowing to `word-bank-server`, which the marketing site reads back for its word wall — plus any sentence you explicitly ask the AI to analyze.

**Where it runs:** the site and the web app are static builds on Netlify. `wiktapi.dev` and `word-bank-server` run as containers on a single DigitalOcean droplet, reached through a Cloudflare Tunnel — the droplet accepts no inbound connections, so both services are bound to loopback and the tunnel is the only route in.

## Built with

- **App** — Expo SDK 55, React Native, TypeScript, Expo Router, NativeWind, AsyncStorage
- **Site** — Astro (static), Tailwind CSS v4, TypeScript
- **Server** — Node, Express, and Node's built-in `node:sqlite`
- **Dictionary API** — Nitro, better-sqlite3, a single SQLite file built from Wiktionary dumps
- **Data** — Wiktionary via [kaikki.org](https://kaikki.org) (definitions), [Open Library](https://openlibrary.org) (books), [Datamuse](https://www.datamuse.com/api/) (English autocomplete), [Groq](https://console.groq.com/) (AI, with [Cerebras](https://cloud.cerebras.ai/) as a second free tier it falls back to on a rate limit)

## Run it yourself

Each repo has its own README with full setup; the short version:

```bash
# Dictionary API — needs a wiktionary.db built first (see its README)
cd wiktapi.dev      && pnpm install && pnpm --filter @wiktapi/api run dev

# Word bank REST API — word feed + AI; set GROQ_API_KEY to enable the AI routes
cd word-bank-server && npm install && npm run dev

# Marketing site
cd word-bank-site   && npm install && npm run dev

# The app — needs a custom dev client, it does not run in Expo Go
cd word-bank-app    && npm install && npm run dev
```

Point the app and site at your own servers with the `EXPO_PUBLIC_*` / `PUBLIC_*` environment variables documented in each repo's `.env.example`.

## Links

- **Website** — https://wordbankapp.com
- **Web app** — https://app.wordbankapp.com
- **App** — https://github.com/wordbank-project/word-bank-app
- **Site** — https://github.com/wordbank-project/word-bank-site
- **Server** — https://github.com/wordbank-project/word-bank-server
- **Dictionary API** — https://github.com/wordbank-project/wiktapi.dev

## License

<!-- TODO: use appropriate LICENSE -->
[MIT](./LICENSE)
