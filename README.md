# Wobbly Kitchen

A sandbox cooking game for a 7-year-old. No timer, no score, no losing.
Prep the food, cook it, make it look nice, serve it, wash up.

No build step, no dependencies, no server. Just open it.

---

## Play it right now

Double-click `index.html`. That's the whole thing.

## Put it on a tablet

The game is a proper installable web app — it gets a home-screen icon, opens
full-screen with no browser bars, and works with no internet at all.

1. Deploy this repo (see below) and open the URL on the tablet.
2. **iPad:** Safari → Share → **Add to Home Screen**.
   **Android:** Chrome → menu → **Install app**.
3. Turn on **Guided Access** (iOS Settings → Accessibility) or **Screen Pinning**
   (Android) so he can't swipe out of it by accident.

### Deploying to Netlify

**From this repo:** on netlify.com → *Add new site* → *Import an existing project*
→ pick this repo. Leave the build command empty and set the publish directory to
`.` (the repo root). Every push to `main` redeploys automatically.

**Or without Git at all:** drag the project folder onto
[netlify.com/drop](https://app.netlify.com/drop) — you get a live URL in seconds.

---

## How the game works

The chef walks along the counter. Move him with the **arrow keys** or the
on-screen **◀ ▶** buttons; the big green button does whatever makes sense at the
station he's standing at, and it always says what that is.

Everything can also just be dragged with a finger or a mouse, so it plays fine
without ever touching the chef.

```
 ① PREP  →  ② COOK  →  ③ PLATE  →  ④ SERVE          (and then CLEAN)
  board       pan        plate      customer
```

| Ingredient | Prep step | How |
|---|---|---|
| 🥕 🥔 🧅 🍅 🍄 | **CUT** | swipe across the board, or press the button ×3 |
| 🥚 | **CRACK** | press ×2 |
| 🍋 | **SQUEEZE** | press and hold until the ring fills |

The board holds three things at once and one press preps **all** of them.

**Nothing he makes is ever wrong.** Unknown combinations get a silly generated
name, confetti and a place in the recipe book. Burnt food is a joke, never a
punishment. The recipe counter only ever goes up.

Three stars from the customer needs all three: prepped properly, cooked properly,
and plated with **MAKE IT NICE**.

---

## Changing things

All the game data sits in one block at the top of the `<script>` in `index.html`.
Adding a recipe is one line — ingredient order never matters:

```js
{ need:["bread","cheese","tomato|cut"], name:"Pizza", emoji:"🍕" },
```

`|cut`, `|crack` and `|juice` mean the prepped version is required. Leave the
suffix off and the whole ingredient works.

Other knobs, all next to each other:

| Constant | Does |
|---|---|
| `STATE_MS` | seconds per cooking stage (raw → cooking → cooked → golden → burnt) |
| `MAX_HANDS` / `MAX_BOARD` / `MAX_PAN` | how many things fit in each place |
| `SCRUBS` | presses to wash the pan |
| `PREP` | which ingredients need prepping, and which gesture |
| `CUSTOMERS` / `VERDICTS` | who turns up and what they say |

**Hidden reset:** press and hold the title for 3 seconds to wipe the saved
recipe book.

---

## Files

| File | |
|---|---|
| `index.html` | the entire game — self-contained, font embedded, opens offline |
| `manifest.webmanifest` | makes it installable to the home screen |
| `sw.js` | offline caching (bump `CACHE` after changing the game) |
| `icon-180.png` / `icon-512.png` | home-screen icons |
| `cooking-game-spec.md` | the original design brief |

Progress saves to `localStorage` under one key, `wobblyKitchen`.
