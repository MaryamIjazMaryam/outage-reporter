 # Outage Reporter

A live neighborhood map where people report power cuts, so everyone can see which areas are affected right now.

**Live demo:** https://maryamijazmaryam.github.io/outage-reporter/

Built for the Global Innovation Build Challenge.

## The problem

When the power goes out, people don't know if it's only their house or the whole area. They call neighbors, check WhatsApp groups, or just wait and guess. They also can't easily decide whether to stay or go somewhere else, such as a friend's house, to finish online work.

## What it does

- Shows a 6 by 6 map of neighborhood blocks (A1 to F6). Yellow means power is on and dark means power is out.
- Tap a block to report a power cut, or to say the power is back.
- Checks the neighboring blocks and tells you whether it looks like an area problem or just your own wiring.
- Lists recent reports with times such as "5 min ago".
- Shows the average length of past outages, as a simple pattern from reports.
- Reports are saved online, so everyone sees the same map. The page refreshes the data every 5 seconds.

## Technologies

- HTML, CSS (CSS Grid) and plain JavaScript
- Supabase (PostgreSQL database) with the supabase-js library
- Row Level Security so anyone can read and add reports but not edit or delete them
- GitHub Pages for hosting

## How it works

1. The page loads all reports from a `reports` table in Supabase.
2. A block's status is the newest report for that block, and it is "on" if there are none.
3. Tapping a block adds a new report to the table, and the map redraws.
4. The neighbor check counts how many blocks directly above, below, left and right are out.

## Run it yourself

1. Download `index.html`.
2. Create a free project at supabase.com and run this SQL in the SQL Editor:

```sql
create table reports (
  id bigint primary key generated always as identity,
  block text not null,
  status text not null check (status in ('on', 'out')),
  time bigint not null
);

alter table reports enable row level security;

create policy "anyone can read reports"
on public.reports for select to anon using (true);

create policy "anyone can add reports"
on public.reports for insert to anon with check (true);

grant select, insert on public.reports to anon;
```

3. In `index.html`, replace `YOUR_PROJECT_URL` and `YOUR_PUBLISHABLE_KEY` with your own Project URL and publishable key from the Supabase dashboard.
4. Open the file with Live Server in VS Code, or upload it to GitHub Pages.

## Limitations

- Anyone can submit a report, so there is no protection against fake reports yet.
- The average outage length is a pattern from past reports, not a real prediction. It improves with more data.
- Free Supabase projects pause after a week without use.

## Next steps

- Add the electricity company's planned cut schedules to warn people before a cut.
- Require several matching reports before a block turns dark, to reduce fake reports.
- Use real street names instead of block codes.
- Add simple sign-in to limit spam.

## Author

Maryam Ijaz, BSSE student, University of Faisalabad
