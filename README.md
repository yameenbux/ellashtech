# èllash — booking page

A one-page appointment booking flow for **èllash**, a mobile lash technician
covering Birmingham, Nuneaton and Coventry.

Open `index.html` — no build step, no dependencies.

## The flow

1. **Treatment** — full sets and infills, grouped and priced as on èllash's own price list.
2. **Date & time** — pick an area, then a date and start time.
3. **Your details** — name, contact, and the address being travelled to.
4. **Confirm** — review, agree to the deposit and cancellation policy, book. The
   confirmation carries èllash's aftercare guide, grouped by when each rule matters.

## Treatments

| | Full set | Infills |
|---|---|---|
| Hybrid  | £45 | £35 |
| Russian | £50 | £40 |

All prices include travel to the appointment. Durations are estimates and are
the one thing here not taken from the client's own material — they set how many
start times fit in a day, so they are worth confirming.

## How availability works

èllash is mobile, so the calendar is built around travel days rather than a
fixed salon diary. Each area is served on set days, and choosing an area
filters the calendar to just those days:

| Area       | Days                  |
|------------|-----------------------|
| Nuneaton   | Mondays & Tuesdays    |
| Coventry   | Wednesdays & Thursdays|
| Birmingham | Fridays & Saturdays   |

Sundays are closed, and bookings need a day's notice. The calendar opens on
the first month that actually has an opening, so it never lands on an empty
view. Start times are filtered so the treatment finishes by 21:00.

## Sample data

This is a working sample, so there is no server. Taken slots come from a
deterministic hash of the date, which keeps the same dates looking the same on
every visit, and confirmed bookings are held in `localStorage` — book a slot
and it shows as taken when you go back. Swapping in a real diary means
replacing `slotTaken()` and `confirmBooking()` with API calls.

## Configuration

Everything a new client needs changing sits in one `CONFIG` block at the top of
the script in `index.html`: notification endpoint and email, deposit amount,
payment link, patch-test lead time, diary code, infill interval.

## Where bookings go

On confirming, the page POSTs the booking to `CONFIG.notifyEndpoint`. If that
fails for any reason, it shows a **Send my booking** button that opens a
prefilled email instead, so a booking is never silently lost. Both paths are
tested.

Two caveats:

- **The claude.ai artifact sandbox blocks outbound POSTs**, so the demo link
  always takes the email fallback. Self-hosted (or anywhere normal), the POST works.
- The relay currently points at a third-party form service. It needs activating
  once by clicking the link in its first email, and it receives customer names,
  addresses and phone numbers — fine for testing, but production needs a proper
  data-processing arrangement or a self-hosted handler.

## The diary

`#diary`, unlocked with `CONFIG.diaryCode`. Lists upcoming bookings with client
contact details, and lets days be closed off — a blocked day leaves the customer
calendar immediately.

The code is client-side and sits in the page source. It keeps a casual visitor
out; it is not security. Anything genuinely private needs a server.

## Patch tests

Selecting *"First time — never had lashes"* requires a patch test
`CONFIG.patchTestHours` before the appointment. If the chosen slot is sooner
than that, the booking is blocked with an explanation. The requirement is
carried into the review screen, the confirmation and the booking email.

## Still to confirm with the client

- **Appointment durations** — estimated, not supplied.
- **The £10 deposit and 48-hour cancellation terms** — placeholders. The wording
  lives in `stepConfirm()` and the `DEPOSIT` constant.

## Photos

Each treatment carries its own close-up, matched to the treatment by the
client's own Instagram captions — "Pretty hybrids" to the hybrid full set,
"Russians on this doll" to the Russian, and so on. Hybrids read visibly
wispier than the Russians side by side, which is the point: it is the only
honest way to explain what the extra £5 buys.

They are cropped square on the lash line (Instagram chrome removed), resized
to 240px and embedded as data URIs — about 12 KB each, 50 KB in total, so the
page stays self-contained with no external requests.

To swap one, replace the `photo:` value on that treatment in `SERVICES`. A
treatment with an empty `photo` simply renders without one.

**These are identifiable clients.** They are already public on the business's
own Instagram, but a booking page is a different use, and permission is the
client's to obtain before this goes live.

## Live touches

Everything here is driven by real state — nothing is decorative filler.

- **Flowing eyebrow ticker.** A seamless marquee across the top. The first item
  is computed, not written: it reads the soonest genuinely bookable date across
  every area ("Next availability Mon 31 Aug in Nuneaton"). Content is rendered
  twice so a `translateX(-50%)` loop never seams, matching the
  `matrix(1,0,0,1,-50,0)` transform evidence in the design system. Pauses on hover.
- **A lash line that keeps moving.** After the fan draws itself in, a wave of
  brightness travels along it on a stagger, so the mark is never quite still.
- **Scarcity from real availability.** Days with two or fewer slots left get a
  blush dot and an "Almost gone" key; the chosen date shows "Only 2 left". These
  are counted from the diary, never invented.
- **Slot-hold countdown.** The confirm step shows a ten-minute hold on the slot,
  and says so plainly when it lapses rather than dumping the booking.
- **Counting total.** The price rolls to its new value instead of snapping.
- **Hover lift** on treatment and area cards, a **staggered reveal** of the
  treatment list, a **pulsing step marker**, and a slow **sheen across the main
  button** — the design system's `translateY(-15.4px)` and outline evidence,
  scaled to something tasteful.

Every one of these is disabled under `prefers-reduced-motion: reduce`.

### Deliberately not included

No "Chloe booked 2 hours ago" pop-ups. Fabricated past bookings shown to real
customers are a dark pattern, and this page has no way to know they are true.
The scarcity above says the same thing honestly, because it is counted from
actual free slots.

## Design

Spacing, corner radii and the type scale come from the supplied design tokens.
The palette is èllash's own olive-gold on a warm near-black, with the tokens'
dusty rose used for soft surfaces. Type is Italiana for display and Jost for
body and figures.

Light and dark are both supported — the page follows the viewer's system
setting, and the Theme button overrides it. All text meets WCAG AA.
