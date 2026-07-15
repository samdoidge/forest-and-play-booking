# Forest & Play — Holiday Club Booking Form

Single-file static booking form for Forest & Play (forest school / holiday club, Winslow, Buckinghamshire). It replaces a 12-page PDF booking pack. Built July 2026.

**Live:** https://book.forestandplay.org (HTTPS enforced)

## Architecture — the whole chain

- `index.html` — the entire app: inline CSS + JS, no build step, no dependencies.
- Hosted on **GitHub Pages** from this repo (`main` branch, root). Push to `main` → live in ~1 minute.
- `CNAME` file pins the custom domain `book.forestandplay.org`. Do not delete it.
- DNS: the main site forestandplay.org runs on **Webador** (site-builder, Pro plan) with a wildcard `*.forestandplay.org` → Webador. An explicit CNAME record `book` → `samdoidge.github.io` (added in Webador's DNS settings, registrar backend is Openprovider) overrides the wildcard for this one subdomain.
- The main Webador site links here via a nav item "Holiday Club Booking". The form is deliberately separate from the site's enquiry/signup forms.

## Submissions — Web3Forms

- Delivery: email to **hello@forestandplay.org** via Web3Forms (account owned by the nursery, signed up with that address).
- The `access_key` in index.html is **public by design** (client-side key; it can only send mail to the account's inbox).
- Free tier: client-side POSTs only (curl/server-side requires their Pro plan), and **submissions are stored on Web3Forms servers for 30 days** — the nursery accepted this trade-off knowingly (the form carries child medical data).
- The Web3Forms dashboard has a per-form "website" setting that must match the host serving the form. It's set to the live domain; **submissions from localhost will be rejected** unless you temporarily flip that setting.

### ⚠️ Hard-won gotcha: the "password" field name

Web3Forms' spam filter **silently rejects any submission containing a field NAMED "password"** (any name containing the word) with the unhelpful error *"Could not submit this form for security reasons"*. Field **values** containing "password" are fine. This cost significant debugging time. The pickup-password field is therefore named `Collection code word` internally while the visible label still says "Collection password". Never rename it back; never add a field with "password" in its name.

Other error decoder: `"Please use POST method"` = someone opened the API URL directly; 400 + "security reasons" = spam filter (check field names first).

## Form behaviour (all inline JS in index.html)

- **Session dates**: the `WEEKS` array at the top of the script (marked "EDIT HOLIDAY DATES HERE") drives the whole grid. One entry per week, only running days listed (part-weeks fine). When dates change: edit `WEEKS` **and** the "Summer 2026: …" chip in the header `<div class="meta-strip">`, push.
- Pricing: full day £65, half day (morning/afternoon) £35 — appears in the table header, summary maths, and header chip; change all together.
- Ticking a full day auto-unticks that day's half sessions and vice versa; morning+afternoon on one day shows a "full day is cheaper" hint.
- Email subject + a "Booking summary" field are auto-built at submit ("Holiday Club booking — {child} (N sessions, £X)").
- **Empty fields are stripped** from the FormData before send so emails only show filled fields; deliberate "No" radio answers still send.
- **Autosave**: every input mirrors to localStorage (`fp-booking-draft`), restored on load, cleared only on successful submit. Added after a submission failure ate a full manual fill — do not remove.
- Honeypot: hidden `botcheck` checkbox (Web3Forms convention).
- The paper form's "office use" section was intentionally omitted; duplicate questions from the PDF (happy/worries asked twice) were merged.

## Design

- Palette and art come from the original PDF booklet ("Holiday Club Booklet.pdf"): cream `#FDFAF5`, forest greens, orange `#C75B28`. Page background, card background, and the art's baked-in cream **must all stay `#FDFAF5`** or the feathered sticker edges show as squares.
- `art/*.png` are crops from the booklet's full-page JPEGs (each PDF page is one flat image), given feathered alpha edges with PIL. Corner foliage top of page, one animal per section header, trees flanking the footer.
- Headings: system serif stack (Iowan Old Style/Palatino/Georgia). No webfonts.

## Testing recipe

- Logic (totals, exclusivity, reveals, autosave) is testable on any local server — but **submission from localhost is domain-blocked** (see above).
- A real submission test = a real email to hello@forestandplay.org. Always put "TEST" in the child's name and tell the nursery to delete it.
- Playwright/automation CAN submit successfully (their bot filter is content-based, not automation-based).

## People / ops

- Site owner: Forest & Play nursery; Becca (staff) reviews the form and gave the "add real dates" feedback.
- Sam Doidge (sam@samdoidge.com, GitHub samdoidge) built and maintains it; repo is on his personal account — transfer if handing off.
- Future growth path: if the club ever needs capacity limits ("Tuesday is full") or payments, graduate to a proper booking system; this form's field structure ports over.
