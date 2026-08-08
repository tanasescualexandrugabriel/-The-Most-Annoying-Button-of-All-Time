# The Most Annoying Button of All Time

A small joke web game: a "Click for 1M$" button that dodges your cursor (and your finger, on mobile) every time you get close to it. It looks completely unbeatable with a mouse or a touchscreen but there's exactly one way to actually win.

Built with plain HTML, CSS and JavaScript, plus Bootstrap 5, Animate.css and canvas-confetti loaded from a CDN.

## How the game works

- The button starts centered on screen under the title.
- The moment your mouse hovers over it (or your finger touches it on mobile), it teleports to a new random spot on screen, with a little wobble animation.
- Every dodge increases the "Missed attempts" counter.
- At 5, 12, 20 and 35 missed attempts, the button changes text and color to taunt you a bit harder, with a screen flash.
- **The actual win condition:** press `Tab` to move keyboard focus around the page until the button gets focus (you'll see a yellow outline around it), then press `Enter` or `Space`. A keyboard-triggered click never fires the "hover" event that makes the button run, so it's the one input method that always lands.
- On a win: confetti, a short screen shake, and a message showing how many attempts it took you to figure out the trick. A "Play again" button resets everything.

## Files in this project

### `index.html`
The page structure. Contains:
- The centered title, hint text and missed-attempts counter.
- The `#crazy-btn` button (the one that runs away).
- The hidden `#success-alert` box that appears when you win.
- `<link>` tags for Bootstrap CSS, Google Fonts (Poppins) and Animate.css, plus a link to the local `style.css`.
- `<script>` tags for canvas-confetti and Bootstrap JS (loaded from CDN), followed by the local `utils.js` and `script.js`. The order matters: `utils.js` must load before `script.js` because `script.js` calls functions defined in `utils.js`.

### `style.css`
All the visual styling:
- An animated diagonal gradient background.
- `.game-wrapper` uses `pointer-events: none` so the background never blocks clicks, while `#game-ui` and `#success-alert` re-enable pointer events for the text and buttons that need to be interactive.
- The glowing title style, the counter pill style, and the button's base look and position.
- `#crazy-btn:focus-visible` — the yellow outline that appears only when the button is reached via keyboard. This is not just decoration: it's the only clue you get for where the button is when you're using `Tab` instead of a mouse, which is what makes the keyboard win path actually playable.
- The `flash-white` and `quake-effect` keyframe animations used for the "evolution" taunts and the win moment.

### `utils.js`
Two small helper functions used by `script.js`:
- `calculeazaPozitieNoua(latimeButon, inaltimeButon, margine)` - picks a random on-screen position for the button that always stays fully inside the visible viewport, with a safety margin so it never spawns half off-screen.
- `obtineEvolutieButon(scor)` - returns a new text and Bootstrap color class for the button once the missed-attempts count hits certain thresholds (5, 12, 20, 35). Returns `null` when there's no upgrade at the current score.

### `script.js`
The main game logic:
- Grabs references to all the DOM elements it needs by `id`.
- `fugiDeCursor()` — runs on both `mouseover` and `touchstart`. Increments the score, asks `utils.js` for a new safe position, moves the button there, plays the wobble animation, and applies a button "evolution" if the score just crossed a threshold. Both mouse and touch are wired to this function so the game is equally hard to win on desktop and mobile.
- The `click` listener on the button — this only fires when a click reaches the button without a prior `mouseover`/`touchstart`, which in practice means it was triggered from the keyboard. This is the win condition: it hides the game UI, shows the success alert, fires the confetti, and fills in how many attempts it took.
- `reseteazaJocul()` — resets the score, button text/class/position and UI visibility back to the starting state. It's wired to the "Play again" button and also exposed on `window` in case you want to trigger it from an inline `onclick` instead.

## How to download and open the project

1. **Download the ZIP** of this repository (on GitHub: the green **Code** button → **Download ZIP**).
2. **Unzip it** anywhere on your computer:
   - **Windows:** right-click the downloaded `.zip` file → *Extract All...* → choose a folder → *Extract*.
   - **macOS:** double-click the `.zip` file it extracts to the same folder automatically.
   - **Linux:** right-click → *Extract Here*, or from a terminal:
     ```bash
     unzip the-most-annoying-button-of-all-time.zip
     ```
3. **Open the folder in VS Code** (or any code editor):
   - In VS Code: `File` → `Open Folder...` → select the unzipped folder.
   - Or from a terminal, inside the unzipped folder:
     ```bash
     code .
     ```
   - Any other editor works too (Sublime Text, WebStorm, Notepad++, etc.) - just open the folder.

## How to run it

You have two options:

**Option A — just open the file (simplest):**
Double-click `index.html`, or right-click it and choose *Open with* → your browser. Since the game only reads local files (no server-side code, no build step), this works fine on its own.

**Option B — run a local server (recommended, avoids some browser quirks):**
- With VS Code: install the **Live Server** extension, then right-click `index.html` → *Open with Live Server*.
- With Python installed, from inside the project folder:
  ```bash
  python3 -m http.server 8000
  ```
  then open `http://localhost:8000` in your browser.

No installation, build tools, or dependencies are required — everything else (Bootstrap, fonts, Animate.css, confetti) loads from a CDN when the page opens, so you'll need an internet connection the first time you run it.

## Tip for testers

If you can't catch the button with your mouse (you won't), try pressing `Tab` a few times and watching for the yellow focus ring then hit `Enter`.
