# Build Spec: "Wobbly Kitchen" — a sandbox cooking game for a 7-year-old

Build a single self-contained `index.html` file (inline CSS + JS, no build step, no
dependencies, no server). It must open by double-clicking on a laptop and also work
on a tablet browser.

## The player

One child, age 6–8. Reads short simple words only. No timer, no score, no losing.
The whole point is: **combine things, see what happens, feel clever.**

If you ever have to choose between "more game" and "more delightful reaction to the
thing he just did" — choose the reaction.

---

## PHASE 1 — Build this first and stop

Do not build phases 2 and 3 until phase 1 is playable end to end. He is going to play
it after phase 1, and what he does will change what we build next.

### Screen layout (one screen, no scrolling, landscape)

```
┌──────────────────────────────────────────────────────┐
│  WOBBLY KITCHEN                    📖 7    🔊         │
├───────────────┬──────────────────────────────────────┤
│               │                                      │
│   PANTRY      │        🔥  [ the pan ]               │
│   (scrolls)   │            heat: ▁▃▅                 │
│               │                                      │
│  🍅 🥕 🥔     │        🍽️  [ the plate ]             │
│  🥚 🧀 🍞     │                                      │
│  🍗 🐟 🍄     │     [ SERVE IT ]      [ 🗑️ ]        │
│  🧅 🌽 🍋     │                                      │
└───────────────┴──────────────────────────────────────┘
```

### Core loop

1. **Drag an ingredient** from the pantry into the pan (or straight onto the plate).
2. The pan shows what's in it — up to 3 items, drawn as overlapping emoji.
3. **The pan cooks over time.** Each item in the pan advances through states on a
   timer: `raw → cooking → cooked → golden → burnt`. Show this with a color filter
   and a wobble, plus rising steam particles. About 4 seconds per state.
4. **Tap the pan** to tip its contents onto the plate.
5. **SERVE IT** → the dish is evaluated, named, and reacted to. Then the plate clears.

### The reaction (this is the most important part of the game)

On serve, run the contents through a recipe table:

- **Known combo, cooked well** → the real dish name appears in big letters
  ("PIZZA!"), confetti bursts, a happy chime plays, and if it's new it gets added
  to the recipe book with a "NEW RECIPE!" banner.
- **Known combo, but burnt** → a black smoking blob, a comedy cough sound, and a
  silly name: "Extra Crispy Pizza." Still gets a happy reaction. Burnt is a joke,
  never a punishment.
- **Unknown combo** → generate a funny name from the ingredients and call it a
  discovery too: "Cheesy Fish Surprise", "Mystery Goo". Give it the same confetti.
  **Nothing the player makes is ever wrong.**

Every served dish counts. The counter in the corner only goes up.

### Recipe table (start with these, all 2–3 ingredients)

| Ingredients | Dish |
|---|---|
| 🍞 + 🧀 | Grilled Cheese |
| 🍞 + 🧀 + 🍅 | Pizza |
| 🥚 + 🧀 | Cheesy Omelette |
| 🥚 + 🍞 | French Toast |
| 🥔 + 🧅 | Hash Browns |
| 🍅 + 🧅 + 🥕 | Soup |
| 🍗 + 🍋 | Lemon Chicken |
| 🐟 + 🍋 | Fish Dinner |
| 🥕 + 🥔 + 🧅 | Stew |
| 🌽 + 🧀 | Corn Cheese |
| 🍄 + 🧅 + 🧀 | Mushroom Melt |
| 🍞 + 🍗 + 🧀 | Big Sandwich |

Order of ingredients must not matter. Compare as a sorted set.

### Silly-name generator for unknown combos

Pick a random template and fill it with the ingredient names:
`"{A} Surprise"` · `"Wobbly {A} {B}"` · `"{A}-{B} Mash"` · `"Mystery Goo"` ·
`"Chef's Special {A}"` · `"Super {A} Stack"`.

### Input requirements — critical

- Use **Pointer Events** (`pointerdown` / `pointermove` / `pointerup`), not mouse
  events, so drag works identically with a finger and a mouse.
- `touch-action: none` on draggable elements so the tablet doesn't scroll mid-drag.
- Every tap target at least **64×64 px**. Small targets are the number one way a
  game like this fails for a 7-year-old.
- Also allow **tap-to-add** as well as drag: tapping an ingredient sends it to the
  pan. Some kids won't discover dragging.

### Sound

Generate with the Web Audio API — no audio files. Five sounds:
sizzle (filtered noise while the pan is hot), plop (item added), ding (served),
sparkle (new recipe), cough (burnt). Add a mute toggle, on by default.

---

## PHASE 2 — Only after he's played phase 1

- **Recipe book** (the 📖 button): a grid of all recipes. Discovered ones show the
  dish; undiscovered show `???` with the ingredient count as a hint. This turns the
  game into a treasure hunt, which is the main reason to keep playing.
- **The chopping board**: drag 🥕 🥔 🧅 onto a board, then swipe or tap across it to
  cut. Show real slices appearing with each cut. Chopped ingredients unlock a second
  set of recipes (chopped 🥔 = Fries).
- **The taste tester**: a friendly animal character sits at the counter. He does not
  order anything and never leaves. He just reacts to whatever gets served — big
  chewing animation, then a face. He loves everything, but has funny specific
  favorites ("MORE CHEESE PLEASE").

---

## PHASE 3 — Stretch

- Mixing bowl with a stir gesture (hold and circle; a ring fills).
- Second burner so two things cook at once.
- A "make your own recipe" screen where he names a dish himself and it's saved
  permanently into the recipe book. **Ask the child for these names, not the AI.**
- Chef hat / apron color picker.

---

## Visual direction

Not a slick app. Think **paper cut-out craft**: thick hand-drawn-looking borders,
everything slightly rotated off-axis (1–2°), and a satisfying squash-and-stretch
wobble on every single interaction. The signature move is that nothing in this
kitchen sits perfectly straight, and everything bounces when touched.

- Palette: warm butter yellow background, tomato red, leaf green, deep brown for
  outlines, and cream for surfaces. High saturation, high contrast.
- Type: one heavy rounded display face for dish names (a chunky Google Font like
  Baloo 2 or Fredoka, loaded from CDN, with `sans-serif` fallback if offline), and
  system sans for the few small labels.
- Ingredients and dishes are **emoji at large size** — no image assets to make or
  load, and they render on every device. Rely on scale, rotation, drop shadow, and
  motion to make them feel like objects rather than text.
- Animate: items arc into the pan rather than teleporting; the pan shakes while
  sizzling; the dish name scales up with an overshoot bounce.
- Respect `prefers-reduced-motion`.

## Copy rules

Every word on screen must be readable by a 6-year-old. Use `SERVE IT`, not `Submit`.
Use `NEW RECIPE!`, not `Recipe unlocked`. No sentences longer than four words
anywhere in the UI. No error messages exist in this game.

## Technical notes

- Save discovered recipes and settings to `localStorage` under one key.
- Include a hidden reset: a long-press on the title for 3 seconds clears saves.
- Must fit in the viewport at 1024×768 and on a tablet in landscape without
  scrolling. Use `dvh` units and `clamp()` for sizing.
- Keep all game data (ingredients, recipes, name templates) in plain JS objects at
  the top of the script so a parent can add new ones by hand in 30 seconds.
