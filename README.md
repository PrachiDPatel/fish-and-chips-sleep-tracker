# Sleep Tracker

A simple mobile companion to Pleco, my LED catfish.

It logs sleep and wake times. The background is a sky that follows the time of day.

**Demo:** this repo is hosted on GitHub Pages. All data here is fake and stays in your browser. The "Send to Pleco" toggle points at a demo MQTT topic, so nothing real lights up. The real app is private.

## How it works

Two buttons: Sleep when you go to bed, Awake when you get up.

Logging at 2 AM counts toward the previous day. You can also rate tiredness, log meals, and leave a note. There is a week view and a doctor report that exports CSV.

## Pleco

Pleco is a paper-mache catfish with LED strips. Logging Sleep switches it to its night preset over MQTT; logging Awake switches it back to morning. There is a toggle to turn that off.

## Tech

One HTML file, vanilla JS, no framework. The real version runs on a Cloudflare Worker with SQLite. This demo fakes the backend in memory.

## Links

- Demo: https://prachidpatel.github.io/fish-and-chips-sleep-tracker/
- Repo: https://github.com/PrachiDPatel/fish-and-chips-sleep-tracker
