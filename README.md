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

## Still to confirm with the client

- **Appointment durations** — estimated, not supplied.
- **The £10 deposit and 48-hour cancellation terms** — placeholders. The wording
  lives in `stepConfirm()` and the `DEPOSIT` constant.

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
