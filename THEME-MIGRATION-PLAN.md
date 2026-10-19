# yogawithpeter.com — Theme Migration Plan

Surveyed 2026-10-18 via wp-admin.

## Current state

- WordPress 7.1.3, theme **Twenty Seventeen Child** (child has no real customizations:
  empty style.css, boilerplate functions.php, unmodified page.php copy).
- Homepage = latest posts (no static front page). Header = full-bleed featured photo +
  site title/tagline. Right sidebar widget = Google Calendar embed ("Schedule").
- Content: 9 pages (About, Contact, Playlist, Policies, Privacy Policy, Private
  Instruction, Props, Schedule, Testimonials), 29 published posts + 4 drafts,
  all in "Uncategorized". Pages are plain text/images — nothing theme-specific.
- Menu "Top Menu": Home / About (Playlist, Policies, Props) / Testimonials /
  Schedule (Schedule a Class) / Private Instruction / Contact (Schedule a Class) /
  Privacy Policy. Also a Social Links Menu location.
- Plugins (8, all theme-independent): Contact Form 7 + Flamingo, Duplicator Pro
  (daily backups, 90 kept), InfiniteWP client, Safe Redirect Manager, Sucuri,
  Two Factor, WP Rollback. reCAPTCHA on CF7.
- Site Health: 3 recommendations (missing PHP module, outdated SQL server, no
  object cache). Not blockers.

## Why this is an easy migration

Nothing is locked to Twenty Seventeen. The only theme-coupled items are:
1. Header image + tagline (Customizer).
2. Sidebar widget with the Google Calendar iframe.
3. Menu location assignments.
4. Any "Additional CSS" in the Customizer (check before switching).

## Recommendation: Kadence (free)

- Both candidates are fine. Kadence is lighter, in the wp.org repo (auto-updates,
  WP Rollback works), and its free tier covers everything here. Blocksy starter
  sites need the Blocksy Companion plugin and nudge toward Pro.
- Use a Kadence starter site only as a visual reference, or skip it: 9 pages is
  faster to restyle by hand than to untangle a starter site's demo content.

## Steps

1. **Staging copy.** Duplicator Pro → build a package → install to a staging URL
   (host staging feature, or a subdomain like staging.yogawithpeter.com).
   Never test a theme on production.
2. **Capture current config** on production before touching anything:
   - Download header image from Customizer → Header Media.
   - Copy any Customizer → Additional CSS.
   - Copy the Google Calendar embed code from the Schedule widget.
   - Screenshot home, About, Schedule, Contact for comparison.
3. **On staging:** install Kadence, activate. Install "Kadence Blocks" (free) for
   the front page layout.
4. **Rebuild the shell:**
   - Customizer → Header: logo/title, tagline, assign Top Menu + Social menu.
   - Kadence Header Builder: transparent header over a hero image to mimic the
     current look, or a simpler solid header.
   - Footer: Privacy Policy link, copyright.
   - Sidebar/footer widget: re-add the Google Calendar embed (Custom HTML block).
5. **New static front page** (decided 2026-10-18): a normal block page with a hero
   image + one-line pitch, "Book a class" button to the booking page, the calendar
   embed, and the core "Latest Posts" block showing the newest 3 posts (no extra
   plugin needed). Set Reading → static page. Create a "Blog" page, set it as the
   Posts page, add it to the menu and link it under the Latest Posts block.
6. **Check each page** on staging (9 pages, 2 min each). Contact Form 7 form and
   reCAPTCHA should work unchanged.
7. **Mobile check** on phone width for home, Schedule, Contact.
8. **Cut over:** Duplicator Pro package of staging → restore over production
   (or repeat steps 3–5 on production by hand, which takes ~1 hour given this
   site's size). Take a fresh production backup first either way.
9. **Post-switch:** Flush any cache, resubmit sitemap in Search Console, check
   Safe Redirect Manager rules still resolve, run Sucuri scan, re-run Site Health.
10. Keep Twenty Seventeen installed for 2 weeks as a rollback, then delete it
    and the child theme.

## Rollback

Appearance → Themes → activate Twenty Seventeen Child. Menu and widget
assignments may need re-pointing; everything else is untouched.

## Open items to decide

- Kadence vs. Blocksy (plan assumes Kadence; steps are identical otherwise).
- Where staging lives (host-provided vs. subdomain).

## Captured from Twenty Seventeen (2026-10-18, before switch)

- Additional CSS: empty.
- Header images: randomized set in Customizer → Header Media (one is
  `/wp-content/uploads/2019/03/cropped-7D124AE0-603E-4B49-A500-55088B7458E1-1.jpeg`).
- Site icon: `/wp-content/uploads/2018/09/cropped-man-in-lotus-position-...png`.
- Sidebar "Schedule" widget embed:
  `<iframe style="border-width:0" src="https://calendar.google.com/calendar/embed?showTitle=0&showPrint=0&showTabs=0&showCalendars=0&mode=AGENDA&height=400&wkst=1&bgcolor=%23FFFFFF&src=qbgu5b8hf5lq78i4u525m8bgmo%40group.calendar.google.com&color=%23875509&ctz=America%2FNew_York" width="400" height="400" frameborder="0" scrolling="no"></iframe>`

## Done on live, 2026-10-18

- Kadence (free) installed and activated. Twenty Seventeen + child kept for rollback.
- Kadence Blocks NOT installed: core blocks were enough.
- New page "Home" (ID 9379): full-width cover hero + title/tagline + "Book a class"
  (Calendly) and "Private instruction" buttons; Latest Posts block (3) with
  "See all posts" link; Google Calendar embed. Kadence page meta: title hidden,
  layout fullwidth, content unboxed, vertical padding off.
- Hero rotation: an HTML block on Home with a tiny inline script that picks one of
  7 Twenty Seventeen header crops at random on each load (same images as before).
- New page "Blog" (ID 9381) set as Posts page; Reading → static front page.
- Top Menu: "Blog" added at the end; duplicate auto-added items removed;
  "auto add pages" turned off.
- Footer credit changed to "© {year} Yoga With Peter · Privacy Policy".
- Checked: Home, Blog, About, Contact (CF7 form renders), Schedule, a single post.

## Rollback

Appearance → Themes → activate Twenty Seventeen Child, then Settings → Reading →
"Your latest posts". Home/Blog pages can stay or be trashed.

## Still to do / optional

- Phone-width check in a real phone (window resize in the automation did not
  take effect; Kadence and core columns are responsive by default).
- Customizer polish: fonts, colours, logo, social links in header/footer.
- Consider dropping "Privacy Policy" from the top menu (now in footer).
- Delete Twenty Seventeen + child after ~2 weeks.

## Visual refresh, 2026-10-18 (later)

- Fonts: Playfair Display (headings, site title), Inter (body, nav). Google Fonts via Kadence.
- Palette (Kadence global palette): sage #7A8B6F accent, deep sage #5E6D54, ink #2E2924,
  body #4A433D, muted #857C73, sand #DED5C8, light sand #F1EAE0, cream #FAF7F2, white.
- Pill buttons (40px radius). Cream site background, white content, sand page-title bar,
  dark footer.
- Transparent header enabled globally but forced OFF on every page except Home via the
  per-page `_kad_post_transparent` meta (Kadence's "pages" toggle alone didn't stop it).
  Home hero has a dark top gradient so white nav text stays readable on light photos.
- Home sections: hero (rotating photo) → Ways to practise (3 cards) → Where to find me
  (Medford / Malden / Online) → testimonial → About teaser with headshot → Upcoming
  classes (calendar card) → From the blog (3-col grid).
- Section backgrounds use inline `var(--global-paletteN)`; Kadence does not emit
  `has-paletteN-background-color` classes for core blocks.
- Copy in the three-card sections and the About teaser is placeholder text written by
  Claude; Peter should edit to taste.

## Fixes, 2026-10-18 (evening)

- Home page block markup regenerated through the editor's own serializer; every block
  now validates (no "unexpected or invalid content" warnings). "Ways to practice".
- Gotcha: `wp.blocks.serialize()` emits core/html blocks as empty `<!-- wp:html /-->`,
  so the rotation script and calendar iframe must be spliced into the raw content as
  text, not created as blocks. Editing other blocks in the editor UI is fine.
- "Where to find me" corrected per Peter: Medford = The Corner Studio (379 Main St,
  link verified in browser), Malden = Ascension Self Defense (no link; none exists in
  site content), Online = Private Instruction + yogawithhollandandpeter.com (verified 200).
- About teaser rewritten from the About page's own sentences (2011 injury, functional
  approach). Removed the invented "came to yoga in my 30s".
