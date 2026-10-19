# yogawithpeter.com — maintenance notes

WordPress site for Peter's yoga teaching. This repo holds documentation only;
the site itself lives in WordPress at https://yogawithpeter.com/wp-admin/.
Nothing here is deployed anywhere.

History of the 2026-10 theme migration is in [THEME-MIGRATION-PLAN.md](THEME-MIGRATION-PLAN.md).

## Stack

| Item | Value |
|---|---|
| WordPress | 7.1.3 (auto-updates on) |
| Theme | Kadence (free, from wordpress.org). No child theme. |
| Old theme | Twenty Seventeen + Twenty Seventeen Child, still installed for rollback. Delete after ~2 weeks (early Nov 2026). |
| Block plugins | None. Core Gutenberg blocks only. Kadence Blocks is **not** installed. |
| Plugins | Contact Form 7 + Flamingo, Duplicator Pro (daily backups, 90 kept), InfiniteWP client, Safe Redirect Manager, Sucuri, Two Factor, WP Rollback, reCAPTCHA on CF7 |

## Where everything lives

All customisation is in the database, not theme files, so Kadence updates are safe.

| What | Where |
|---|---|
| Colours, fonts, header, footer, button shape | Appearance → Customize (Kadence) |
| Home page layout and copy | Pages → Home (ID 9379) |
| Blog listing | Pages → Blog (ID 9381), set as Posts page in Settings → Reading |
| Tagline | Settings → General |
| Header menu | Appearance → Menus → "Main" (location: Primary) |
| Footer text | Customize → Footer → Footer HTML |
| Calendar feed | Google Calendar `qbgu5b8hf5lq78i4u525m8bgmo@group.calendar.google.com` |
| Booking link | https://calendly.com/paradigmpetern/60minyoga |

## Design tokens (set in Customizer → Colors / Typography)

- Palette: 1 `#7A8B6F` sage (accent), 2 `#5E6D54`, 3 `#2E2924` ink, 4 `#4A433D` body,
  5 `#857C73` muted, 6 `#DED5C8` sand, 7 `#F1EAE0` light sand, 8 `#FAF7F2` cream, 9 `#FFFFFF`.
- Headings + site title: Playfair Display (title weight 600). Body + nav: Inter (nav 500).
- Buttons: 40px radius (pill).
- Transparent header: enabled globally, disabled per page on every page except Home
  (Kadence page sidebar → "Transparent Header" → Disable). New pages need this set
  or the nav will be white on a light background.

## Home page structure (Pages → Home)

1. Cover hero: eyebrow "MEDFORD · MALDEN · ONLINE", headline, tagline, buttons
   "Book a class" (Calendly) and "See the schedule" (`#schedule`).
2. **HTML block** right after the cover: a script that swaps the hero image for one
   of 7 photos at random on each load. Do not delete it.
3. Ways to practice: Yin / Restorative / Hatha & Meditation cards.
4. Where to find me: Medford (The Corner Studio) / Malden (Ascension Self Defense) /
   Online (yogawithpeter.com, yogawithhollandandpeter.com).
5. Testimonial.
6. "Hi, I'm Peter" with headshot.
7. Upcoming classes: **HTML block** with the Google Calendar iframe (anchor `schedule`).
8. From the blog: core Latest Posts block, 3 columns.

Per-page Kadence settings on Home: title hidden, layout full width, content unboxed,
vertical padding off.

### Editing the Home page

- Edit text and cards in the block editor as normal. Every block validates.
- To change hero photos, edit the HTML block's image list (paths under
  `/wp-content/uploads/`).
- If you ever regenerate blocks programmatically, note `wp.blocks.serialize()` outputs
  HTML blocks as empty `<!-- wp:html /-->`. The rotation script and calendar iframe
  must be pasted back as raw text. Editing in the normal UI does not have this problem.
- Section backgrounds use inline `background-color: var(--global-paletteN)`; Kadence
  does not generate palette background classes for core blocks.

## Routine tasks

- **Add a post:** Posts → Add New. Appears on Blog and in the Home "From the blog" grid.
- **Add a page:** Pages → Add New, set Transparent Header to Disable in the Kadence
  sidebar, then add to Appearance → Menus → Main if wanted (auto-add is off).
- **Change the schedule:** edit the Google Calendar; the site embeds it live.
- **Change the booking link:** Home page hero button, plus the Schedule page.
- **Back up Customizer settings:** Appearance → Kadence → Export Theme Settings.
  Duplicator Pro also runs daily full backups.
- **Theme or plugin update goes wrong:** Plugins → WP Rollback on the item, or restore
  a Duplicator package.

## Rollback to the old theme

1. Appearance → Themes → activate Twenty Seventeen Child.
2. Settings → Reading → "Your latest posts".
3. Re-assign the Main menu to the Top Menu location if needed.
Home and Blog pages can stay or be trashed.

## Known gaps

- Phone-width layout was not verified in a real phone; Kadence and core columns are
  responsive by default but worth a look.
- Site Health flags a missing PHP module, an outdated SQL server, and no object cache.
  Host-side, not blockers.
