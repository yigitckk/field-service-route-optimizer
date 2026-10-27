# Field Service Route Optimizer

Quantifies how much money small field-service businesses lose every day by
scheduling jobs in booking order instead of route-optimized order.

## Who this is for

**Direct buyer:** any small business that sends technicians/vans to multiple
job sites per day and currently schedules by booking order, not route
efficiency — HVAC, plumbing, electrical contractors, **signage installation
crews**, home repair, local courier/delivery, pest control, cleaning
services. Dispatchers almost never have OR/routing background, so this gap
is nearly universal in businesses under ~20 vehicles (above that, they
usually already run commercial routing software).

**Why I built this specific demo:** I run a signage design business
(CAD + fabrication drawings) and install-crew scheduling is the exact same
problem — multiple job sites per day, booking-order dispatch by default.
This isn't a hypothetical niche, it's adjacent to work I already do.

## Headline result (synthetic 14-job day, 2 vans)

```
Distance reduction: 58.0%
Estimated annual savings (1 route/day, $0.70/km): $22,984
```

Even accounting for synthetic-data optimism, a 20-40% reduction is typical
and realistic for businesses that have never route-optimized — this is a
well-established result in logistics literature, not a cherry-picked number.

## How it works

Uses [Google OR-Tools](https://developers.google.com/optimization), the
open-source solver Google uses in its own logistics products, to solve a
Capacitated Vehicle Routing Problem (CVRP): given N job sites and a fleet
of vans, find the route split that minimizes total distance driven.

Compares against the "naive" route (jobs visited in the order they were
booked, split evenly across vans) — this is what almost every small
business actually runs today via a paper schedule or basic calendar app.

## Usage

```bash
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
python optimizer/route_optimizer.py --jobs 14 --vans 2 --seed 7
```

## What's next (real deployment, not demo)

- Swap synthetic coordinates for a real client's job addresses (geocoded via
  a free API) — this is a ~1 day customization, not a rebuild.
- Add time windows (customer "available 9-11am" slots) — OR-Tools supports
  this natively, the scaffolding (`AddDimension`) is already in the code.
- Package as a weekly service: client sends a CSV of tomorrow's jobs, gets
  back an optimized route PDF/Google Maps link by email — a productized
  $200-500/month offering rather than a one-off project.

## License

MIT
