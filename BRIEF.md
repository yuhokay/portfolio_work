# Bryan Luna, portfolio site

Design spec and build brief. `index.html` is the approved prototype. Match it, then wire in real images.

## What the site is

Bryan Luna, creative director and photographer, New York. Five chapters of a career, each a place he has worked rather than a discipline. Portico House has its own separate site, so this one is the individual credential.

The site argues he is a **creative director who shoots**, not a photographer who sometimes directs. Copy and role labels should never quietly demote direction.

## Architecture

Single page, client-side views. No framework.

- **Home** is an index and nothing else. No project rows. Name and statement in an animated stage, then five section columns, then five panels, then a scrolling client ticker.
- **Section pages** hold that section's projects in full. Reached from the nav, a column, a panel, or the next-section link at the foot of the previous one.
- Sections in order: Portico House, Flok, Highsnobiety, Photography, Blühill.

All content lives in the `SECTIONS` array at the bottom of `index.html`. That array is the content model. Adding a project is one object in it, nothing more.

## Deliberate decisions, do not "improve" these

- **No years anywhere.** Much of the strongest work is older and chronology broadcasts age. Do not add a timeline, a recent-work sort, or year labels.
- **Home shows no projects.** The whole point of the rebuild. Do not surface project rows on the index.
- **The stage has nine fixed collage slots.** Positions are hardcoded so the composition is designed. Only which fragment shows, and what is in it, changes. Random placement looks like a bug within seconds.
- **Slot widths range from 8% to 27% on purpose.** A few large, a few small, nothing average. Evening them out is what made the page feel flat.
- **A fragment takes the shape of the image it is showing**, from `RATIOS`, so nothing is cropped. Do not put fixed aspect ratios back on the slots.
- **No image appears twice at once.** A shared `inUse` set claims whatever is on screen and releases it on hide.
- **The client ticker pauses on hover** and holds still for reduced motion. Names come from the `CLIENTS` array.
- **Every fragment sits behind the name.** Both layer classes resolve to the same z-index specifically so a future fragment cannot cover it.
- **Fragments hard cut, they do not fade.** 60ms transition, each on its own timer starting at 150ms. The reference cuts.
- **Only one thing moves.** The five panels are single still images. If the panels start cycling too, the stage stops reading.
- **Cycling runs on home only** and is cleared entirely when a section opens.
- **Columns and panels share identical grid tracks.** The columns grid has no side padding and no column gap, text is inset 18px, and the outer two columns take the page gutter. This is what makes each label sit over its own panel. Do not add padding or gaps back.
- **Columns use subgrid** so name, descriptor and discipline list always start level regardless of wrapping.
- **Section names and descriptors are nowrap** above 900px, at 10.5px/.10em and 9.5px/.07em, which is what it takes to fit the track.
- **The masthead nav never wraps to a second line.** It is nowrap and scrolls if it must. Mark, nav and contact hold one row down to 700px, measured rather than guessed; below that the nav takes its own full-width row and the mark carries the auto margin so contact stays right.
- **Uppercase on the nav is set on the buttons, not the container.** Browsers do not reliably inherit `text-transform` into form controls. Do not "tidy" it back up to `.nav`.
- **The first project in a section runs full bleed.** Lead position drives it, it is not a per-project flag. Ordering a section is therefore an editorial decision.
- **No wheel interception on the image rails.** An earlier version converted vertical wheel to horizontal scroll and trapped the page. Vertical scroll must always scroll the page. `overscroll-behavior-x` only, never the shorthand, and `touch-action: pan-y`.
- **Rows open one at a time** within a section.
- **Masthead height is measured at runtime** into `--barH`. Do not hardcode it.
- **Contact is a mailto, not a form.** Every reference site in the moodboard does this.
- **Metadata is the design.** Role, format, client and frame count on every row in mono caps. Role marker shapes: square photography, diamond creative direction, circle founding.
- **History writes are wrapped in try/catch** because sandboxed previews have a null origin and throw.

## Type and colour

- Display: Syne 600/800. Body: Instrument Sans 400/500. Mono: IBM Plex Mono 400/500 for all metadata.
- Paper `#FFFFFF`, ink `#111111`, accent violet `#5B4BFF`.
- White was chosen after comparing warm off-white, near-black, and tinted lilac, sand, sage, blue-grey and blush, twice, the second time with real photographs in. The gallery argument for off-white is about reflected light and carries less weight on an emissive screen; white gives the strongest separation at the edge of an image.
- All reading text is weight 500. Short label/value pairs are uppercase; running prose is not.

## Open, and worth revisiting with real images in

- **Fonts.** Syne is placeholder-ish. Pangram Pangram's PP Neue Montreal and PP Eiko are free for personal portfolio use per their FAQ, paid from around $40 otherwise. Not on Google Fonts, so self-host with `@font-face` and read the EULA first.
- **Cut speed.** 150ms plus 43ms per slot is tuned to a reference made of flat graphic artwork. Photographs carry more information per frame and will likely need slowing.

## What is still placeholder

- **Every image.** `IMGS` currently holds nine paths into `images/stage/`; the files are not there yet. `RATIOS` holds their width/height and must match the real files, ideally read from the images rather than hardcoded. `PANEL_PICK` maps the five section panels to indices in the same array; the panels may want their own set, since they are cropped to a tall frame while the stage fragments are not.
- Contact email (`hello@example.com`) and Instagram handle (`@handle`).
- Flok: both rows, and the only section with fewer than three projects.
- Photography: all three rows. Real personal editorials, portraits and creative shoots are coming.
- Blühill: all three rows are guesses at the shape.
- Highsnobiety: "Editorial feature" and "Digital story".
- Portico House: real client names, placeholder descriptions.
- The logo mark is a detailed pencil drawing that turns to mush at 28px. It needs a simplified small-size version; the full drawing could run large elsewhere.

## Copy that is approved, use verbatim

- Statement, one centered line under the name: *I direct campaigns, commercials, album covers & editorials.*
- Highsnobiety: *Work from my time as sole photographer for the brand, shooting for site, social, and print while producing and pitching ideas of my own, including a few video series.*
- Blühill: *My own label. Tracksuits, and everything around them: identity, lookbooks, editorials, all of it directed and shot end to end.*
- Photography: *Personal work. Editorials, portraits, and creative shoots.*
- About, two paragraphs: *Most of the job is deciding how someone should be seen, then building what it takes to make that picture. Sometimes that's me and a camera, more often a crew assembled for the job. Having done every role myself is why I know who to hire and what to ask of them.* / *I learned that at Highsnobiety as the only photographer on staff, where you produce and pitch your own ideas or they don't happen. Now it runs through Portico House for brand work, Flok for music and culture, and Blühill, which is mine alone.*
- The About copy deliberately shows he can lead a crew rather than only shoot solo. Do not edit it back toward lone-operator language.
- Portico House and Flok standfirsts are not final.
- No em dashes anywhere in copy.

## Build tasks

1. Replace every placeholder with real `img` tags: `srcset` at multiple widths, `loading="lazy"`, explicit `width`/`height` so nothing shifts on load.
2. Read images from a folder, e.g. `images/<section>/<project>/01.webp`, so adding photographs is not a markup edit.
3. The stage collage needs its own curated set. Those images are seen small and for a fraction of a second, so they want strong, legible frames rather than subtle ones.
4. Each panel needs one strong still that represents its section.
5. Export around 2400px long edge for full-bleed leads, 1600px for rail frames, smaller for stage fragments. WebP. Masters outside the repo.
6. Wire the real contact email and Instagram handle.
7. Keep it a single page. If it needs a framework later, `SECTIONS` is the thing to port.
