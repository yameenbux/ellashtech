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

## Design

Spacing, corner radii and the type scale come from the supplied design tokens.
The palette is èllash's own olive-gold on a warm near-black, with the tokens'
dusty rose used for soft surfaces. Type is Italiana for display and Jost for
body and figures.

Light and dark are both supported — the page follows the viewer's system
setting, and the Theme button overrides it. All text meets WCAG AA.
