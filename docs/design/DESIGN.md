# pac-track — Design

Reference for building the UI. Tokens live in [`tokens.css`](tokens.css). Open the mockups in [`mockups/`](mockups/) in a browser to see the decisions below in place. All people, groups, bills and figures in the mockups are examples.

## Stack

- **Nuxt UI + Tailwind CSS.**
- Light and dark modes, following the system setting by default, with a manual toggle.

## Gestalt

- Neutral and content-first. The site's frame is ink, paper and one brass accent. Photos and figures carry the color.
- Modern, with a touch of patriotism carried by the display type and the star mark. No eagles, flags, or stock imagery.
- **Minimal on-screen text.** Labels are short. Explanations, data stamps (date + filing ID) and definitions go in ⓘ popovers attached to the thing they describe. If a line of text can move into a popover, it does.
- Equal treatment: every person gets the same layout, crop, size and color weight.

## Type

| Role | Face | Use |
|---|---|---|
| Display | Libre Caslon Display | Page titles, initials on cards without a photo, dialog titles. Used sparingly. |
| Text and headings | Public Sans | Everything else. Headings at weight 800, tracking -0.02em. |
| Reference | IBM Plex Mono | Filing IDs and data stamps inside popovers. |

Numbers use `tabular-nums`.

## Shape and chrome

- Radius 16px for cards and dialogs, 10px for buttons and inputs. Pills are fully rounded.
- Depth comes from soft shadows, not heavy borders. Cards carry a light shadow that deepens on hover.
- Header: translucent surface with background blur and a hairline bottom border. The wordmark sits beside a brass star in a dark rounded tile.
- Focus ring: 3px brass, offset 2px.

## Color roles

Each color has one job. Party color never doubles as interface or status color.

| Nuxt UI color | Role | Used for |
|---|---|---|
| `primary` | Ink | Buttons, selected pills, figure bars, PAC avatars, the current page's icon tile |
| `neutral` | Cool slate | Text, borders, surfaces |
| `secondary` | Brass | Focus ring, star mark, issue icons, hover ring on avatars. Never a large fill. |
| `error` | Vermilion | Errors, always with icon and text. Faint tint marks "Opposed". |
| `warning` | Amber | Stale data, pending re-verification |
| `success` | Teal green | Approved, verified. Faint tint marks "In favor". |
| `info` | Ink 2 | Neutral notices |
| none (custom) | Party | Party chip and the line under the photo. Nothing else. |

**Party colors:** Democratic blue and Republican red have equal lightness, so neither reads as louder. Every other party uses neutral slate. A party color always appears with the party letter and name.

Links are underlined ink.

## Page shell

Mockup: [`mockups/app-shell.html`](mockups/app-shell.html).

- **Header** (sticky): star mark + wordmark, search, then icon-only theme buttons (system, light, dark).
- **Search** is visible at the top of the page. Once the page is scrolled down, the field hides and a search icon button takes its place. The icon button and the `/` key bring the field back.
- **Search results** drop down from the field, grouped as People, Donors, Committees and Bills. People show a party-colored initial and their office. A 5-digit zip or a street address adds a "Races for…" result at the top.
- **Sidebar**, top to bottom:
  - Nav: Officials & Candidates · Races · Donors · Issues · Bills & Votes · Lobbying · Disclosures & Trades. Each item has an icon in a rounded tile. The current page is a raised surface pill with an ink tile.
  - **Your issues**: the reader's chosen issues, each with a brass icon, and a button that opens the issue picker.
  - Collapse button. Collapsed, the sidebar shows icon tiles only (the choice is saved in the browser).
- Under 960px the sidebar becomes a slide-out drawer opened from a menu button, and the chosen issues show as chips above the page content with the same picker button.
- **Footer**: Methodology · Coverage · Corrections, and the license.
- **Admin**: no sign-in link anywhere for readers. Admins go to `/admin` directly.

## Race card

Used on race pages, side by side. Order from top to bottom:

1. **Photo**, full width and square (up to 360px tall), with a dark gradient over its lower half and a 4px party-color line along its bottom. On the photo, in white: the **name**, then a party chip (letter + name) and a status pill (Incumbent, Challenger, Former). With no portrait on file, the same space shows the person's initials in the display face on a dark backdrop. The photo area looks the same in light and dark mode.
2. **Two figures**, one per row, never added together:
   - *From PACs, 2026*: PAC and party contributions to the campaign.
   - *Outside spending for*: Schedule E spending supporting the candidate. Outside spending opposing them is in this figure's popover.

   Each row has the label and ⓘ on the left, the amount on the right, and an ink bar underneath. The bar shows the amount against the largest amount for that figure in the race. Each figure's ⓘ popover holds its data date and filing ID.
3. **PAC support by issue**: one small heading, then one row per issue the reader chose:
   - Top line: issue name + ⓘ, and the candidate's record tag on the right. The tag has a ✓ and a success tint for Voted for, Cosponsor and Supports; a ✕ and an error tint for Voted against and Opposes; a dashed outline for No position.
   - Second line: the bill the row is anchored to.
   - Two joined boxes: **✓ In favor** (faint success tint) and **✕ Opposed** (faint error tint), each holding an avatar group of PACs that took that side of the bill. An empty side shows a dash.
4. **Full record** row with an arrow, across the bottom of the card.

**PAC avatars:** inverse of the page theme (ink circle, paper initials) for contrast against the tints. Up to 4, then `+N`. Hover or tap opens a popover with the PAC's name, its documented position and source, its direct contribution, and its outside spending for or against the candidate. Each amount has its own data date and filing ID. A brass ring marks the open or hovered avatar. Initials by default; logos only where we have rights to use them.

## Issue picker

- Shown on a reader's first visit, before the first race or official page.
- Up to 3 issues. The 12 v1 lenses appear in Pew rank order, with the next tier under "More issues".
- **Skip** uses the top 3 from the Pew ranking.
- Choices are saved in the URL and in the browser. No account.
- It opens as a dialog. The picker button in the sidebar's "Your issues" block (or beside the issue chips on small screens) reopens it.

## Copy rules

Follow the neutral-copy rules in `CLAUDE.md`. In addition:

- Use the year alone ("2026"), not "2026 cycle".
- Describe each side by its stance on a specific bill or position, never by a label for the group.

## Open questions

- **Bill anchor per issue row.** Which bill each issue row uses: (a) chosen by the maintainer with the reason shown, (b) chosen by a published rule, or (c) no single bill. Leaning (b).
- PAC logos: source and usage rights.
