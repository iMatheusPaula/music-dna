# whatday

A tiny web game that helps a group of friends pick **which festival day to go to**, without them knowing that's what they're doing.

The candidates are five days across two festivals:

| Day | Persona | Artists we care about |
|---|---|---|
| Lollapalooza, Friday | Caos de Festival | David Guetta, Sombr, Cage The Elephant, Snow Strippers, Cat Dealers |
| Lollapalooza, Saturday | Groove Underground | Four Tet, Sammy Virji |
| Lollapalooza, Sunday | Rave Sombria | Charli XCX, The Neighbourhood, The Prodigy, Raye, Boys Noize, Marlon Hoffstadt, Acid Asian, Mind Enterprises, Boy Harsher |
| Ame Laroc, Saturday | Sunset House | Alesso, Bob Sinclar, Kungs, Öwnboss, Schillist |
| Ame Laroc, Monday | Trance Eterno | Armin van Buuren |

## How it works

The player thinks they're taking a "musical DNA" test. They listen to short previews, **with no title or artist shown**, and react to each one:

| Reaction | Score |
|---|---|
| 🔥 Solta o som | +2 |
| 😐 Tanto faz | 0 |
| ❌ Tira isso | −1 |

Every track secretly belongs to one festival day. A day's score is the **average** of the reactions to its tracks, so days with more artists don't win just by having more songs.

### Rounds

- **Part 1 (22 tracks):** 4 per day (3 tracks from real DJ sets + 1 artist hit) plus 2 decoys from outside the line-up that don't count. Order is shuffled and every player gets a different random selection.
- **Bonus round (optional, 10 tracks):** exactly 2 hits per day, so every day ends with 6 tracks and the same share of hits (50%). Familiar hits tend to get more 🔥, so keeping that share equal avoids favoring any day.

Halfway through Part 1, a teaser hints at the leading persona to keep people going.

### Result and reveal

1. **DNA screen:** the leading persona and a percentage per persona, plus a short code such as `#38729 · precisão 100%`.
2. **Reveal:** the twist (each song was from a festival day), a verdict, a ranking of the five days, and the list of tracks with where each one came from.

The verdict rules:

- every day averages ≥ 1 → **go to all of them**
- the best day averages ≤ 0 → "maybe festivals aren't your thing"
- the top day leads the second by ≥ 0.5 → **you have to go on day X**
- otherwise → tie between the top two

### Reading a friend's code

The code has one digit (0–9) per day, always in this order:

```
Lolla Fri · Lolla Sat · Lolla Sun · Ame Sat · Ame Mon
```

Higher means they liked that day more. `#38729` means Lolla Saturday (8) and Ame Monday (9) were the favorites.

## Tracks

All tracks live in [`tracks.js`](tracks.js):

```js
{ "d": "ame-seg", "k": "set", "a": "Armin van Buuren & SACHA", "t": "Set Me Free",
  "u": "https://audio-ssl.itunes.apple.com/…m4a", "o": "Tocou nos 2 últimos sets do Armin (ADE + Tomorrowland)" }
```

| Field | Meaning |
|---|---|
| `d` | day: `lolla-sex`, `lolla-sab`, `lolla-dom`, `ame-sab`, `ame-seg`, or `isca` (decoy) |
| `k` | `set` (played in a real recent set), `hit` (artist hit) or `isca` |
| `a`, `t` | artist and title, only shown after the reveal |
| `u` | 30-second preview URL |
| `o` | origin text shown in the reveal |

Where they came from:

- **DJ sets:** recent 2026 tracklists from [1001Tracklists](https://www.1001tracklists.com), preferring tracks played in more than one set and skipping unreleased IDs and set-only mashups.
- **Hits and live acts:** the artists' best-known songs.
- **Audio:** official 30-second previews from the public [iTunes Search API](https://performance-partners.apple.com/search-api). No audio is hosted in this repo.

To add a track, look it up on the iTunes Search API (`https://itunes.apple.com/search?term=<artist>+<title>&entity=song&country=BR`), copy its `previewUrl` and add an entry with the right `d` and `k`.

## Running locally

It's a static page with no build step:

```sh
python3 -m http.server 8000
```

Then open http://localhost:8000. Use headphones; the first track starts when you tap the start button.

## Deploying

Published with GitHub Pages from the `main` branch root: **https://imatheuspaula.github.io/whatday/**

> Don't send this repository to the friends playing: `tracks.js` and this README give the game away.
