# Meatsgiving arcade artwork

Editable SVGs based on the cream newsprint, red ink, and bold outlines in the existing Meatsgiving site.

| Folder | Files | Dimensions |
| --- | --- | --- |
| sprites | Six meats and four vegetables | 48 × 48 |
| card-stills | Four illustrated game moments | 400 × 400 |
| flap-steak | Wing, fork head, fork handle, kitchen wall, repeating ground | 26 × 14, 56 × 110, 20 × 64, 360 × 494, 46 × 46 |
| cleaver-frenzy | Cutting board | 360 × 540 |
| sausage-dash | Four run frames, jump, crash, lettuce hedge, sun, cloud, roadside post | Runner 60 × 100, hedge 330 × 40 |
| prime-cuts | Cow diagram with nine tappable cut shapes | 340 × 210 |

## Placement notes

- Sprite filenames mirror the keys in `SPRITE`. All ten are centered with transparent backgrounds. The steak points right and its wing is a separate file.
- The card stills have dark, quiet lower-left corners for the site's existing number overlays.
- The fork head has a bottom-facing tip. Rotate the head 180° for a fork rising from the ground. Draw the handle first, stretch it along its length, and overlap the neck end of the head by about 2 px at native size.
- Anchor the wing near the right end of its canvas, roughly at (22, 7), for flapping.
- The kitchen wall can repeat along its 360 px width. Repeat the 46 px ground tile at its native width.
- Each runner frame uses the same 60 × 100 canvas and a bottom-center anchor. The jump frame includes a tucked pose, with vertical travel controlled by the game.
- The cow paths have named IDs, labels, and the same `data-i` ordering as the site's cuts array. Both shanks share one path with two subpaths. Inline the SVG or use its paths in the existing diagram to retain per-cut click handling.
