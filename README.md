# Roshambot

Rock · Paper · Scissors against a bot that studies you.

A single-file browser game (`index.html`, no build step). The bot learns your habits as you play and throws the counter.

## How to play

- Click a hand, or press **1 · 2 · 3** (or **R · P · S**). **M** mutes.
- Settings: opponent (Mind reader / Pure random), match length (first to 3, first to 5, endless), pace, hand skin, sound.
- The side panel ("the bot's notebook") shows how well it reads you, the habits it found, your throw mix and the strategies it weighs.

## How the bot works

- **Six predictors** each guess your next throw: favorite throw, what follows your last throw, two-throw combos, reaction to win/loss/draw, reaction to the bot's last throw, and step habit (cycling forward/back).
- Predictors are weighted by recent accuracy (exponential weights with decay), then blended into one forecast.
- The bot plays the counter with 50–88% probability depending on how confident it is, otherwise random, so it is hard to exploit outright.
- **Fair play:** before you throw, its move is sealed with a SHA-256 hash of `move:nonce`. After the reveal the hash is checked, so it can't switch moves after seeing yours.
- Memory (last 400 throws) is kept in `localStorage` in your browser only.

## Simulated results (300 rounds × 40 games per strategy)

| Player strategy | You win | Bot wins | Draw |
|---|---|---|---|
| Pure random | 33% | 32% | 34% |
| Always rock | 4% | 92% | 4% |
| Rock 50% of the time | 20% | 59% | 21% |
| Cycle R → P → S | 5% | 91% | 4% |
| Win-stay / lose-shift | 4% | 91% | 5% |
| Copy the bot's last throw | 4% | 92% | 4% |
| Counter the bot's predicted move | 76% | 12% | 12% |

Takeaway: any fixed pattern gets crushed; random play is a coin flip. The bot has no "you are predicting me" predictor, so a player who models the bot's own logic can beat it.

## Run locally

Open `index.html` in a browser.
