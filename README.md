# Monterio Youth

Source for the Monterio Youth website, a static site hosted on GitHub Pages.

## Structure

- `index.html` – home page
- `styles.css` – site styles
- `.nojekyll` – tells GitHub Pages to serve the files as-is without Jekyll

## Local preview

Open `index.html` in a browser, or run a simple server from the repo root:

```sh
python3 -m http.server 8000
```

Then visit http://localhost:8000.

## Email sign-up form

The form on the home page writes directly to a Supabase table from the browser
using the project's publishable key (safe to expose; it only grants what
row-level security allows). Settings live at the top of the inline script in
`index.html`: `SUPABASE_URL`, `SUPABASE_KEY`, and `TABLE`.

Expected table and policy (run in the Supabase SQL editor):

```sql
create table public.signups (
  id          bigint generated always as identity primary key,
  email       text not null unique,
  created_at  timestamptz not null default now()
);

alter table public.signups enable row level security;

-- Anonymous visitors may insert, but never read, update, or delete.
create policy "public can sign up"
  on public.signups for insert
  to anon
  with check (true);
```

A hidden "website" field acts as a honeypot: if a bot fills it, the page shows
a success message but sends nothing.

## Deploying

GitHub Pages serves from the `main` branch root. Push to `main` and the site
updates automatically. In the repo settings, under **Pages**, set the source to
**Deploy from a branch**, branch `main`, folder `/ (root)`.
