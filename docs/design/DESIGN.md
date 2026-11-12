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

- Radius 10px for cards, boxes, buttons and popovers. Pills are fully rounded.
- Header: surface background with a hairline bottom border, and a brass star mark beside the wordmark.
- Focus ring: 3px brass, offset 2px.

## Color roles

Each color has one job. Party color never doubles as interface or status color.

| Nuxt UI color | Role | Used for |
|---|---|---|
| `primary` | Ink | Buttons, active nav, selected pills |
| `neutral` | Cool slate | Text, borders, surfaces |
| `secondary` | Brass | Focus ring, star mark, hover ring on avatars. Never a large fill. |
| `error` | Vermilion | Errors, always with icon and text. Faint tint marks "Opposed". |
| `warning` | Amber | Stale data, pending re-verification |
| `success` | Teal green | Approved, verified. Faint tint marks "In favor". |
| `info` | Ink 2 | Neutral notices |
| none (custom) | Party | Party chip and the line under the photo. Nothing else. |

**Party colors:** Democratic blue and Republican red have equal lightness, so neither reads as louder. Every other party uses neutral slate. A party color always appears with the party letter and name.

Links are underlined ink.

## Page shell

Mockup: [`mockups/app-shell.html`](mockups/app-shell.html).

- **Header** (sticky): star mark + wordmark, then search.
- **Search** is visible at the top of the page. Once the page is scrolled down, the field hides and a search icon button takes its place. The icon button and the `/` key bring the field back.
- Hiding and showing search never moves the page. On wide screens the field fades in place. On narrow screens it sits on its own row below the header bar, outside the page flow, with a fixed space reserved at the top of the page; it slides up behind the bar to hide and back down to show.
- **Search results** drop down from the field, grouped as People, Donors, Committees and Bills. People show a party-colored initial and their office. A 5-digit zip or a street address adds a "Races for…" result at the top.
- **Sidebar**: Officials & Candidates · Races · Donors · Issues · Bills & Votes · Lobbying · Disclosures & Trades. Icons and labels; the current page is an ink pill. The bottom of the sidebar holds the theme buttons (system, light, dark) and, on desktop, the collapse button. Collapsed, it shows icons only and the theme buttons stack (the choice is saved in the browser). Under 960px it becomes a slide-out drawer opened from a menu button. The drawer has its own header with a close button and the logo, in the same place as in the page header.
- **Footer**: Methodology · Coverage · Corrections, and the license.
- **Admin**: no sign-in link anywhere for readers. Admins go to `/admin` directly.

## Race card

Used on race pages, side by side. Order from top to bottom:

1. **Photo**, full width, 4:3, with a 4px party-color line along its bottom. With no portrait on file, the same space shows the person's initials in the display face.
2. **Name**, then a party chip (letter + name) and a status pill (Incumbent, Challenger, Former).
3. **Two figures**, side by side, never added together:
   - *From PACs, 2026*: PAC and party contributions to the campaign.
   - *Outside spending for*: Schedule E spending supporting the candidate. Outside spending opposing them is in this figure's popover.

   Each figure's ⓘ popover holds its data date and filing ID.
4. **PAC support by issue**: one small heading, then one row per issue the reader chose:
   - Top line: issue name + ⓘ, and the candidate's record tag on the right (Voted for, Voted against, Cosponsor, Supports, Opposes, No position).
   - Second line: the bill the row is anchored to.
   - Two boxes: **✓ In favor** (faint success tint) and **✕ Opposed** (faint error tint), each holding an avatar group of PACs that took that side of the bill. An empty side shows a dash.
5. **Full record** link.

**PAC avatars:** inverse of the page theme (ink circle, paper initials) for contrast against the tints. Up to 4, then `+N`. Hover or tap opens a popover with the PAC's name, its documented position and source, its direct contribution, and its outside spending for or against the candidate. Each amount has its own data date and filing ID. A brass ring marks the open or hovered avatar. Initials by default; logos only where we have rights to use them.

## Issue picker

- Shown on a reader's first visit, before the first race or official page.
- Up to 3 issues. The 12 v1 lenses appear in Pew rank order, with the next tier under "More issues".
- **Skip** uses the top 3 from the Pew ranking.
- Choices are saved in the URL and in the browser. No account.
- A "Change issues" button in the issue bar above the cards reopens it.

## Copy rules

Follow the neutral-copy rules in `CLAUDE.md`. In addition:

- Use the year alone ("2026"), not "2026 cycle".
- Describe each side by its stance on a specific bill or position, never by a label for the group.

## Open questions

- **Bill anchor per issue row.** Which bill each issue row uses: (a) chosen by the maintainer with the reason shown, (b) chosen by a published rule, or (c) no single bill. Leaning (b).
- PAC logos: source and usage rights.
