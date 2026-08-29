# èllash — booking page

A one-page appointment booking flow for **èllash**, a mobile lash technician
covering Birmingham, Nuneaton and Coventry.

Open `index.html` — no build step, no dependencies.

## The flow

1. **Treatment** — lash sets, infills, removal and BIAB nails, with prices and durations.
2. **Date & time** — pick an area, then a date and start time.
3. **Your details** — name, contact, and the address being travelled to.
4. **Confirm** — review, agree to the deposit and cancellation policy, book.

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

## Design

Spacing, corner radii and the type scale come from the supplied design tokens.
The palette is èllash's own olive-gold on a warm near-black, with the tokens'
dusty rose used for soft surfaces. Type is Italiana for display and Jost for
body and figures.

Light and dark are both supported — the page follows the viewer's system
setting, and the Theme button overrides it. All text meets WCAG AA.
