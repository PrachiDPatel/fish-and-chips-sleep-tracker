# Sleep Tracker (portfolio demo)

I was sleeping badly and spreadsheets weren't helping, so I built this. The sky in the background follows the actual time of day. That's the whole pitch.

**Live demo:** this repo is on GitHub Pages. Open it on your phone and add it to your Home Screen.

This is the portfolio build. Everything runs in the browser with sample data, and nothing leaves your device. The "Send to Pleco" toggle publishes to a demo MQTT topic, so no real hardware lights up. The version I actually use is private: same frontend, real backend (Cloudflare Worker + D1), real data.

## What it does

Two taps to log a night. The home screen shows a live clock and one line: if you fall asleep now, this is when you wake up. Tap Sleep at bedtime, Awake in the morning.

Logging at 2 AM counts toward yesterday's night. The app sorts out the "is it still tonight" question so you don't have to at 2 AM.

At bedtime you can rate tiredness (1-5), log meals (0-4+), and leave a note. Those describe the day you just lived, so they attach to your most recent finished night. Never to the sleep you're about to have.

7-day and 30-day summaries with averages and consistency. A doctor report button downloads a printable summary plus CSV.

It also talks to a gift I built: a paper-mache whale shark strung with addressable LEDs, driven over MQTT. Logging Sleep flips it to its night preset, Awake flips it to morning, automatically, with a kill switch in Settings. In this demo the messages go to a demo topic.

The private build sends Web Push reminders (11 PM for bedtime, 9:30 AM for wake time). Push needs a real backend, so the demo skips it.

## How it's built

One `index.html`. No build step, no framework. Vanilla JS, CSS custom properties for the day-phase theming. The day/night transition holds each palette solid and morphs fast at dawn and dusk. The first version blended continuously and turned to mud halfway through. Readable beat clever.

The private backend is a Cloudflare Worker on D1 (SQLite): a sessions table, an append-only edit history, VAPID Web Push on a cron schedule, and device auth through single-use setup codes instead of pasted tokens. Server-side validation on writes.

This demo is generated from the real frontend by a build script. The API layer is swapped for an in-memory stub with sample sessions, auth is skipped, push is dropped, and the MQTT topic is retargeted to `wled/demo/api`. Same pixels, zero access to anything real.

## Try it

1. Log tab, Sleep. It stamps the current time.
2. Awake in the morning to close it.
3. Answer the bedtime questions and watch them land on the finished night, not the open one.
4. Summary tab for the week view. Doctor report button for the export.
5. Settings, Send to Pleco toggle, watch the automation pause.

Reload and it resets. It's a demo, not a diary.

## Why two versions

The private one holds my real sleep data and controls real hardware. This one has neither and can't reach either. Same engineering, no access. That rule isn't negotiable.

---

Built with vanilla JS and an unreasonable attachment to how 5:47 AM looks.
