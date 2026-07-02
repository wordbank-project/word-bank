# Word Bank

**Turn the books you read into vocabulary you keep.**

Word Bank is a reading companion. Track what you want to read, are reading, or have read — and every time a word stops you, save it with its definition, your own sentence, and your notes. Your books and words live **on your device**, work **offline**, and need **no account**.

<!-- TODO: replace with the live marketing-site URL -->
[![Download for Android (beta)](https://img.shields.io/badge/Download-Android%20(beta)-208AEF?logo=android&logoColor=white)](https://wordbank.app)

_iOS — coming soon · Web — coming soon_

## 📦 Source code

| Repository | What's inside |
| --- | --- |
| **[word-bank-app](https://github.com/wordbank-project/word-bank-app)** | The Expo / React Native app (Android today, iOS & web on the way) |
| **[word-bank-site](https://github.com/wordbank-project/word-bank-site)** | The marketing / showcase site (Astro, bilingual EN/NL) |
| **[word-bank-server](https://github.com/wordbank-project/word-bank-server)** | The community word feed that powers the site's live "word wall" + stats |
| **[wiktapi.dev](https://github.com/jensrot/wiktapi.dev)** | Self-hosted multilingual dictionary API (Wiktionary/kaikki data) |

## What it does

- **Track every book you read** — search millions of titles via Open Library (or add a custom book with its own cover photo) and sort them into _Want to read_, _Currently reading_, or _Have read_.
- **A word bank for every book** — each book keeps its own vocabulary; see at a glance how many words it has taught you.
- **Instant, precise definitions** — every meaning fetched at once, with part of speech and IPA pronunciation; search and pick the one that fits, colour-coded by part of speech.
- **Make words stick** — save each word with the sentence you found it in and your own notes; write a review and notes per book.
- **All your words in one place** — the Words List gathers every word from every book; search, filter by part of speech, and sort A–Z, by book, or by most recently added.
- **Read in your language** — definitions powered by Wiktionary data across 100+ languages; English and Dutch are live today.
- **Private & offline** — no account, no cloud, no tracking; everything is stored on your device.
- **Dark mode** included.

## Get started

1. **Download** the Android beta from the [Word Bank site](https://wordbank.app) and install it.
2. **Find a book** — search by title or author, or add a custom book, and tap a reading status.
3. **Add words as you read** — type a word, pick the definition that fits, and anchor it with your own sentence and notes.

That's it.

## Privacy

- **On-device** — your reading list, words, sentences, and notes are stored locally and work fully offline.
- **No account, no cloud, no tracking** — there is no server holding your data.
- **The community feed is opt-in and anonymous** — when enabled, [word-bank-server](https://github.com/wordbank-project/word-bank-server) stores only a saved *word* and its public dictionary data (definition, part of speech, phonetic) to power the marketing site's live word wall. It never receives your reading list, your sentences, or your notes.

## Architecture

```
                          ┌──────────────────────────────┐
                          │        word-bank-app         │  Expo / React Native
                          │   Android · iOS & web soon   │  — on-device, offline
                          └───┬───────────┬───────────┬──┘
            book search       │  defs     │  defs     │  anonymized saved words
        ┌─────────────────────┘     ┌─────┴─────┐     └────────────┐
        ▼                            ▼           ▼                  ▼
  Open Library              wiktapi.dev    dictionaryapi.dev   word-bank-server
  (book catalog)            (100+ langs)   (English)           (feed + stats, SQLite)
                                                                    │
                                                                    ▼
                                                              word-bank-site
                                                        (marketing · live word wall)
```

The app talks directly to the dictionary and book-search services and stores everything locally. The only data that ever leaves your device — and only when the community feed is enabled — is anonymized saved words flowing to `word-bank-server`, which the marketing site reads back for its live word wall and stats.

## Built with

- **App** — Expo SDK 55, React Native, TypeScript, Expo Router, NativeWind, AsyncStorage
- **Site** — Astro (static), Tailwind CSS v4, TypeScript
- **Server** — Node, Express, better-sqlite3
- **Data** — Wiktionary via [kaikki.org](https://kaikki.org) (dictionary), [Open Library](https://openlibrary.org) (books), [dictionaryapi.dev](https://dictionaryapi.dev) (English)

## Run it yourself

Each repo has its own README with full setup; the short version:

```bash
# Dictionary API (multilingual definitions)
cd wiktapi.dev      && pnpm install && pnpm --filter @wiktapi/api run dev

# Community word feed (powers the site's word wall + stats)
cd word-bank-server && npm install && npm run dev

# Marketing site
cd word-bank-site   && npm install && npm run dev

# The app (dev client — see word-bank-app/README.md for the device setup)
cd word-bank-app    && npm install && npm run dev
```

## Links

<!-- TODO: Custom domain -->
- **Website** — https://word-bank-vault.netlify.app
- **App** — https://github.com/wordbank-project/word-bank-app
- **Site** — https://github.com/wordbank-project/word-bank-site
- **Server** — https://github.com/wordbank-project/word-bank-server
- **Dictionary API** — https://github.com/jensrot/wiktapi.dev

## License

<!-- TODO: Different LICENSE -->
[MIT](./LICENSE)
