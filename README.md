# Fish and Chips: Sleep Tracker

A sleep-tracking web app with a sky that follows the actual time of day — dawn breaks rose and gold, afternoons run azure, dusk burns ember, and night settles into navy and stars. Built because I was having trouble sleeping and wanted something calmer than a spreadsheet, and prettier than my doctor's intake forms.

**Live demo:** this repo is published with GitHub Pages — open it on your phone and add it to your Home Screen for the full effect.

> This is the **portfolio build**. It runs entirely in your browser with sample data — nothing is sent anywhere, and the "Send to Pleco" toggle publishes to a demo MQTT topic, so no real hardware is involved. The private version I actually sleep with lives in a separate repo: same engineering, real backend (Cloudflare Worker + D1), real data.

## What it does

- **Log sleep in two taps.** Home shows a live clock and one line: *if you fall asleep now, you'll wake at X.* Tap **Sleep** when you go to bed, **Awake** when you get up.
- **Smart day chips.** Logging at 2 AM counts toward *yesterday's* night — the app handles the "is it still tonight?" question so you don't have to.
- **Tiredness, meals, notes.** At bedtime you can rate how tired you were during the day (1–5), log meals (0–4+), and leave a note. These describe the day you just lived, so they attach to your most recent *finished* night — never to the sleep you're about to have.
- **Summaries that mean something.** 7-day and 30-day views with averages, consistency, and trends — plus a **doctor report** that downloads as a clean printable summary with your data as CSV.
- **Pleco integration.** This started as a companion to a gift I built: a paper-mache whale shark strung with addressable LEDs, driven over MQTT. Logging Sleep flips it to its night preset; logging Awake flips it to morning — automatically, in the background, with a kill switch in Settings. In this demo the messages go to a demo topic so nothing real lights up.
- **Bedtime and morning nudges.** The private build sends Web Push reminders (11 PM: *"time to log bedtime?"*, 9:30 AM: *"what time did you wake up?"*). Push needs a real backend, so the demo skips it — but the chat-reminder flow it pairs with is part of the same system.

## How it's built

One self-contained `index.html` — no build step, no framework. Vanilla JS, CSS custom properties for the day-phase theming, and a day/night interpolation that holds palettes solid and morphs quickly at dawn and dusk (the first version tried to blend continuously and turned to mud mid-transition — readable won over clever).

The private backend is a Cloudflare Worker with a D1 (SQLite) database: sessions table, an append-only `session_edits` audit trail (old entries are edited, never deleted), VAPID-signed Web Push with a cron that fires the two daily reminders, and device auth via single-use setup codes instead of pasted tokens. Server-side validation keeps writes honest (meals must be 0–4, times must parse, history can be locked).

The demo you're looking at is generated from the real frontend by `build_demo.py` in the private repo: the API layer is swapped for an in-memory stub with sample sessions, auth is skipped, push is dropped, and the MQTT topic is retargeted to `wled/demo/api`. Same pixels, zero access to anything real.

## Try it

Open the Pages link, then:

1. Tap **Log** → **Sleep** to start a night (it stamps the current time).
2. Tap **Awake** in the morning to close it.
3. Answer the tiredness/meals questions at bedtime — watch them land on the finished night, not the open one.
4. Check **Summary** for the week view, and hit the doctor-report button for the printable export.
5. Flip the **Send to Pleco** toggle in Settings to see the automation pause.

Everything resets when you reload — it's a demo, not a diary.

## The personal / portfolio split

I keep two versions of my hardware-adjacent projects:

| | This repo (portfolio) | Private repo |
|---|---|---|
| Data | Sample sessions, in-memory | My real sleep data, D1 |
| Backend | None — static page | Cloudflare Worker + D1 |
| Pleco | Publishes to `wled/demo/api` | Controls my actual LED gift |
| Reminders | Skipped | Web Push + chat reminders |

The rule is simple: nothing in a portfolio piece can reach my real devices or my real data. The engineering is the same; the access isn't.

---

Built with vanilla JS and an unreasonable attachment to how 5:47 AM looks.
