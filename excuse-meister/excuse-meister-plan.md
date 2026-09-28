# Excuse-Meister — Mini Prototype Plan

## Top-Level Overview

Build a single-page browser app (pure HTML/CSS/JS, no backend) that generates funny, petty excuses based on a user-selected category. The prototype is intended as a pitch demo. It includes:

- A category selector (Work, School, Social)
- A randomised excuse display card
- An Excuse Strength Meter (visual bar rating from Believable → Petty → Ridiculous)
- A Copy to Clipboard button

No frameworks, no build tools — just one `index.html` file the user can open in a browser.

---

## Sub-Tasks

---

### Sub-Task 1 — Project scaffold (HTML structure)

**Intent**
Create the `index.html` file with semantic markup for all UI sections so subsequent tasks have a stable DOM to style and script against.

**Expected Outcomes**
- File `index.html` exists and opens in a browser without errors
- Page contains: app title, category buttons (Work / School / Social), a Generate button, an excuse output card, a strength meter section, and a Copy button
- No styling or logic yet — just structure

**Todo List**
- [ ] Create `index.html` with HTML5 boilerplate
- [ ] Add heading `Excuse-Meister` and tagline
- [ ] Add three category toggle buttons (Work, School, Social)
- [ ] Add a Generate Excuse button
- [ ] Add a card div for the excuse text output
- [ ] Add a strength meter section (label + bar container + tier label)
- [ ] Add a Copy to Clipboard button

**Relevant Context**
- All code lives in a single file; `<style>` and `<script>` tags go in the same file
- No external dependencies

**Status:** [ ] pending

---

### Sub-Task 2 — Styling (CSS)

**Intent**
Make the prototype visually presentable for a pitch demo — clean, mobile-friendly, with clear hierarchy. The strength meter bar must be visually distinct across its three tiers (green → orange → red).

**Expected Outcomes**
- App looks polished in a browser at mobile and desktop widths
- Category buttons highlight the selected one
- Excuse card is prominent and readable
- Strength meter bar fills and changes colour based on tier
- Copy button is clearly actionable

**Todo List**
- [ ] Add a `<style>` block in `index.html`
- [ ] Style the page: dark or vibrant background, centered layout, max-width container
- [ ] Style category buttons with an active/selected state
- [ ] Style the excuse output card (large text, rounded, shadow)
- [ ] Style the strength meter: labelled bar track with a coloured fill div
- [ ] Style the Copy button
- [ ] Ensure layout is readable on a phone screen (responsive)

**Relevant Context**
- Strength meter tiers: Believable (0–33%), Petty (34–66%), Ridiculous (67–100%)
- Tier colours: green for Believable, orange for Petty, red for Ridiculous

**Status:** [ ] pending

---

### Sub-Task 3 — Excuse data & generator logic (JavaScript)

**Intent**
Populate a curated bank of funny excuses per category and wire up the Generate button to pick one at random and display it.

**Expected Outcomes**
- Clicking Generate with a category selected shows a random excuse from that category's pool
- Each category has at least 8 excuses
- If no category is selected, a prompt nudges the user to pick one
- Re-clicking Generate always shows a different excuse (no immediate repeat)

**Todo List**
- [ ] Add a `<script>` block in `index.html`
- [ ] Define an `excuses` object with keys `work`, `school`, `social`, each holding an array of strings
- [ ] Write at least 8 funny excuses per category (witty, petty, shareable)
- [ ] Track selected category in a JS variable updated by button clicks
- [ ] Implement `generateExcuse()` — picks a random item, avoids immediate repeat
- [ ] Display the excuse in the card div
- [ ] Show a message if no category is selected

**Relevant Context**
- Example style: "I was late because my cat staged a protest against Mondays."
- Excuses should be short enough to read at a glance (one sentence)

**Status:** [ ] pending

---

### Sub-Task 4 — Excuse Strength Meter

**Intent**
After each excuse is generated, assign it a random strength score and render the visual bar with the correct tier label and colour, making the UI interactive and gamified.

**Expected Outcomes**
- Every generated excuse triggers a strength score (random integer 1–100)
- The bar fills to the score percentage
- Bar colour and tier label update to match the tier: Believable / Petty / Ridiculous
- Meter resets/updates on each new generation

**Todo List**
- [ ] Write `updateMeter(score)` function that sets bar width and colour
- [ ] Map score ranges to tier labels and colours
- [ ] Call `updateMeter` inside `generateExcuse()` with a fresh random score each time
- [ ] Animate the bar fill (CSS transition on width)

**Relevant Context**
- Tiers: 1–33 = Believable (green), 34–66 = Petty (orange), 67–100 = Ridiculous (red)
- Bar fill animation: `transition: width 0.4s ease`

**Status:** [ ] pending

---

### Sub-Task 5 — Copy to Clipboard

**Intent**
Let the user instantly copy the generated excuse for use in texts or messages — key for the pitch demo's "one tap and send" story.

**Expected Outcomes**
- Clicking Copy writes the current excuse text to the clipboard
- Button label briefly changes to "Copied!" then resets — giving clear feedback
- Button is disabled / does nothing if no excuse has been generated yet

**Todo List**
- [ ] Implement `copyExcuse()` using `navigator.clipboard.writeText`
- [ ] Toggle button text to "Copied! ✓" for 1.5s then back to "Copy"
- [ ] Disable the button on page load; enable it after first generation

**Relevant Context**
- `navigator.clipboard` requires the page to be served over HTTPS or localhost; note this for demo environment
- Fallback: `document.execCommand('copy')` for older browsers if needed

**Status:** [ ] pending

---

### Sub-Task 6 — Upvote Leaderboard (localStorage)

**Intent**
Add a "Pettiest Excuses" leaderboard so users can upvote any generated excuse and see the community's top-ranked ones. Votes persist across page refreshes using `localStorage`, making the app feel live and gamified — key for the pitch's "Leaderboard of the Pettiest Excuses" hook.

**Expected Outcomes**
- A 👍 Upvote button appears alongside each generated excuse
- Clicking it increments that excuse's vote count and saves to `localStorage`
- A leaderboard section shows the top 5 most-upvoted excuses, ranked with vote counts
- Leaderboard updates immediately after each upvote
- Data survives page reload — votes are read from `localStorage` on init

**Todo List**
- [ ] Add a 👍 Upvote button to the excuse card (disabled until an excuse is generated)
- [ ] Add a Leaderboard section to the HTML (ranked list, togglable show/hide)
- [ ] On page load, read vote data from `localStorage` key `excuseMeisterVotes` (JSON object mapping excuse text → vote count)
- [ ] Implement `upvoteExcuse()` — increments current excuse's count, writes back to `localStorage`, re-renders leaderboard
- [ ] Implement `renderLeaderboard()` — sorts all voted excuses by count descending, displays top 5 with rank emoji (🥇🥈🥉4️⃣5️⃣) and vote count badge
- [ ] Style the leaderboard card to visually separate it from the generator
- [ ] Add a toggle button to show/hide the leaderboard panel

**Relevant Context**
- Vote store shape: `{ "excuse text": voteCount, ... }` stored under `localStorage.excuseMeisterVotes`
- Leaderboard re-renders on every upvote and on page load
- Same excuse can be upvoted multiple times in a session (no per-user restriction needed for prototype)

**Status:** [ ] pending

---

## Delivery

All six sub-tasks produce a single `index.html` file. Open it directly in any browser to run the demo — no server required.
