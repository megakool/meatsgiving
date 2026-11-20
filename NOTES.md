# Meatsgiving — project notes

Carried over from the earlier "Meatsgiving event dashboard" chat (Nov 16–20, 2026).
`index.html` is the full site as published (version 6) at
https://claude.ai/artifact/GXUi2Ua8geRw1R6DnFADCZ.

## Event details
Edit the `EVENT` object near the top of the `<script>` in `index.html`.

- **When:** Friday, November 27, 2026, 7:00 PM
- **Where:** 314 S. Medio Dr.
- **Bring:** An appetite, and a meat or side if you can

## What's on the page
- **meatCAPTCHA gate** — "I'm not a vegetarian" checkbox, then two rounds:
  select all images with *meat*, then with *vegetables*. Passing shows
  "Verified carnivore" and lets the guest in (remembered per device).
- **Invitation** — price-tag card with date, address, what to bring, and a
  countdown chip ("N days until the meat sweats").
- **RSVP** — Going / Maybe / Can't go, name, party size (1–12), what they're bringing.
  "Who's coming" guest list with head count.
- **The Chopping Block** — message board. Must RSVP first; 200 chars per post;
  each guest keeps their 15 most recent; guests can delete their own posts.
- **The Meat Games (puzzles):** Meatle (5-letter meat Wordle), Cold Cuts
  (16 words → 4 groups, 3 rotating puzzles), The Meaty Mini (5×5 crossword
  with solve timer), Prime Cuts (tap the cut on a cow diagram, 9 rounds).
- **The Meat Arcade:** Flap Steak, Cleaver Frenzy, Butcher's Match (goal 800),
  Sausage Dash. Sound effects with an on/off toggle.
- **Leaderboard** — top 10 per game (Flap Steak, Cleaver Frenzy, Butcher's
  Match, Sausage Dash, Prime Cuts).
- Games are locked until the guest RSVPs or enters a name.

## Known limitations / open items
1. **Shared data doesn't work for guests on the claude.ai link.** RSVPs,
   messages and scores only stay on each guest's own device there. To make them
   shared, host the page elsewhere (e.g. GitHub Pages from this repo) and set
   `SYNC_URL` in `index.html` to a free Firebase Realtime Database URL
   (e.g. `https://your-project-default-rtdb.firebaseio.com`). The page then
   reads/writes `guests/<id>.json` and polls every 15 s.
2. **Captcha still uses drawings, not real photos.** Wikimedia rate-limited the
   download. Options discussed: (a) send 12–20 photos (about half meat, half
   veg/decoys); (b) retry Wikimedia slowly (needs a credits line);
   (c) Adobe Stock via the Adobe connector (may cost credits). To use photos, fill
   the `PHOTOS` array, e.g. `{ src: 'captcha/ribeye.jpg', meat: 1, alt: 'Ribeye steak' }`;
   with 9 or more it switches over automatically.
3. Difficulty and sound volume of the arcade games were not tuned by hand —
   adjust if anything feels too hard, too easy or too loud.
4. The earlier Wikimedia approval added `commons.wikimedia.org` and
   `upload.wikimedia.org` to the Default cloud environment's allowed hosts;
   remove them in environment settings if not wanted.
