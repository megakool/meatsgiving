# Meatsgiving — project notes

`index.html` is the whole site: one self-contained page with no build step.

## Design
Rebuilt from the Claude Design project "App redesign and voice refinement"
(`Meatsgiving Final` for phone, `Meatsgiving Desktop` for 900px and wider).

- Cream newsprint, red ink, Gabarito (Google Fonts). Colors are tokens at the top of the `<style>`.
- **Gate:** one captcha round, "select all images with meat".
- **Front page:** masthead, "Return of the Meat", one red invitation card, the reply card
  (shrinks to a confirmation bar after you RSVP), and a tabbed panel for
  Who's coming / Messages. On desktop it splits into two columns.
- **Games page** (`#/games`; old `#/back` links still work): one Games list (Meatle, Meatsgiving Crossword, Flappy Meat, Meat Ninja, Meat Dash)
  and a leaderboard (fastest first crossword solve, plus high scores for the three arcade games). Locked until you RSVP.
- **Game screens** (`#/play/<id>`): ids are `meatle`, `mini` (crossword), `flap`, `slice`, `run`.
- The URL hash tracks the page, so the phone's back button works.

## Edit the party details
The `EVENT` object at the top of the `<script>`: date, time, address, what to bring,
headline, dek, tagline.

- **When:** Fri, Nov 27, 2026, 7:00 PM
- **Where:** 314 S. Medio Dr.

## Crossword
- Edit the puzzle with `GRID` (`#` = black square) and `CLUES` (keys like `A3`, `D8`) in `index.html`.
  Numbering and word extents are worked out from the grid.
- On phones the clue bar and keyboard stay pinned to the bottom and the grid is sized to fit above them.
  On computers with a mouse the on-screen keyboard is hidden (type on the real keyboard; Enter = next clue,
  Space = switch direction).
- Progress and elapsed time are saved on the device until the puzzle is solved. Only each guest's first solve
  goes on the leaderboard.

## Captcha photos
`captcha/` holds 16 photos (8 meat, 8 decoys), cropped square at 320px as WebP, about 400 KB total.
They're listed in `PHOTOS` right under `EVENT`. Each round shows 9 of them, 3 to 5 of which are meat.
To add or swap one, drop the file in `captcha/` and add a line such as
`{ src: 'captcha/ribeye.webp', meat: 1, alt: 'Ribeye steak' }`.

- **Meat:** prime rib, grilled steaks, braised beef, churrasco, pulled pork, glazed beef, sliced steak, Korean beef
- **Decoys:** chickpea stew, black bean burger, pasta, arugula salad, cucumber salad, crispy tofu, silken tofu, avocado toast

## Game art
- `art/` holds the arcade art pack: sprites, card stills, and backgrounds/frames per game.
  `STILLS` (under `EVENT`) maps each game to its card picture. Each piece falls back to a drawn
  version if its file is missing.
- `art/card-stills/butchers-match.svg` and `art/prime-cuts/cow.svg` belong to removed games and are unused.

## Live site
- **Hosting:** Render Static Site at **https://meatsgiving.org** (GoDaddy DNS: `@` A record and `www` CNAME to Render;
  `www` redirects to the bare domain). Also reachable at https://meatsgiving.onrender.com. It deploys from `main` on every push (publish directory `.`, no build command).
- **Shared data:** RSVPs, posts and scores are stored in the Firebase Realtime Database at
  `https://meatsgiving-78270-default-rtdb.firebaseio.com`, set as `SYNC_URL` in `index.html`.
  Guest entries live under `guests/<id>`. The page polls the database every 15 s.
- **Firebase rules:** anyone can read `guests`, and anyone can write a guest entry that has a `name`
  (24 characters or fewer) and a `status`. Nothing else can be written.
  The site has no logins, so someone technical could edit entries.

## Link preview
- `og.jpg` (1200x630) is the image shown when the link is pasted into a text or chat. `apple-touch-icon.png`
  is the tab and home-screen icon.
- The preview tags in `<head>` point at `https://meatsgiving.org/`. If the domain ever changes, update the
  `og:url` and `og:image` addresses.
- The preview image shows the date and time. If they change, regenerate it.

## Starting fresh
To wipe the guest list, messages and leaderboard: delete every entry under `guests` in the Firebase console
(Realtime Database → Data), **and** bump `DATA_RESET` in `index.html` and push. The bump clears each guest's
saved RSVP and scores on their own device; without it, returning guests re-send their old entry.
Last reset: Nov 20, 2026 (`DATA_RESET = 1`).

## Known limitations / open items
- **Copy to confirm** (from the design notes): "It will mostly be ignored", "Return of the Meat".
- **Fonts.** Gabarito stands in for Funday or Widston. Swap it in if you license the real fonts.
- **Arcade tuning.** Difficulty and sound volume were never tuned by hand.
- **Wikimedia hosts.** An earlier session added `commons.wikimedia.org` and `upload.wikimedia.org`
  to the Default cloud environment's allowed hosts. Remove them in the environment settings if
  you don't want them.
