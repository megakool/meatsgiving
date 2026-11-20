# Meatsgiving — project notes

`index.html` is the whole site: one self-contained page with no build step.

## Design
Rebuilt from the Claude Design project "App redesign and voice refinement"
(`Meatsgiving Final` for phone, `Meatsgiving Desktop` for 900px and wider).

- Cream newsprint, red ink, Gabarito (Google Fonts). Colors are tokens at the top of the `<style>`.
- **Gate:** one captcha round, "select all images with meat".
- **Front page:** masthead, "Return of the Meat", one red invitation card, the reply card
  (shrinks to a confirmation bar after you RSVP), and a tabbed panel for
  Who's coming / The Chopping Block. On desktop it splits into two columns.
- **Back page** (`#/back`): four puzzles, four arcade games, leaderboard. Locked until you RSVP.
- **Game screens** (`#/play/<id>`): Meatle, Cold Cuts, The Meaty Mini, Prime Cuts,
  Flap Steak, Cleaver Frenzy, Butcher's Match, Sausage Dash.
- The URL hash tracks the page, so the phone's back button works.

## Edit the party details
The `EVENT` object at the top of the `<script>`: date, time, address, what to bring,
headline, dek, tagline.

- **When:** Fri, Nov 27, 2026, 7:00 PM
- **Where:** 314 S. Medio Dr.

## Captcha photos
`captcha/` holds 16 photos (8 meat, 8 decoys), cropped square at 320px as WebP, about 400 KB total.
They're listed in `PHOTOS` right under `EVENT`. Each round shows 9 of them, 3 to 5 of which are meat.
To add or swap one, drop the file in `captcha/` and add a line such as
`{ src: 'captcha/ribeye.webp', meat: 1, alt: 'Ribeye steak' }`.

- **Meat:** prime rib, grilled steaks, braised beef, churrasco, pulled pork, glazed beef, sliced steak, Korean beef
- **Decoys:** chickpea stew, black bean burger, pasta, arugula salad, cucumber salad, crispy tofu, silken tofu, avocado toast

## Slots waiting on assets
Each slot is a config value right under `EVENT`:

1. **`STILLS`**: optional gameplay stills for the arcade cards, e.g. `{ flap: 'stills/flap.jpg' }`.
   Without them, each card shows a numbered color block.
## Live site
- **Hosting:** Render Static Site at https://meatsgiving.onrender.com. It deploys from `main` on every push (publish directory `.`, no build command).
- **Shared data:** RSVPs, posts and scores are stored in the Firebase Realtime Database at
  `https://meatsgiving-78270-default-rtdb.firebaseio.com`, set as `SYNC_URL` in `index.html`.
  Guest entries live under `guests/<id>`. The page polls the database every 15 s.
- **Firebase rules:** anyone can read `guests`, and anyone can write a guest entry that has a `name`
  (24 characters or fewer) and a `status`. Nothing else can be written.
  The site has no logins, so someone technical could edit entries.

## Known limitations / open items
- **Domain.** When your domain is ready, add it in Render under Settings → Custom Domains.
- **Copy to confirm** (from the design notes): "It will mostly be ignored", "Return of the Meat".
- **Fonts.** Gabarito stands in for Funday or Widston. Swap it in if you license the real fonts.
- **Arcade tuning.** Difficulty and sound volume were never tuned by hand.
- **Wikimedia hosts.** An earlier session added `commons.wikimedia.org` and `upload.wikimedia.org`
  to the Default cloud environment's allowed hosts. Remove them in the environment settings if
  you don't want them.
