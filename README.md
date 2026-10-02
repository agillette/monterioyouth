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

The table is `public.newslettersignups` (created as `newsletterSignups`;
Postgres lowercases unquoted identifiers) with an `email` column. Row-level
security is on. The anonymous role needs an INSERT grant and an insert policy,
and nothing else, so the publishable key can never read the list:

```sql
grant insert on public.newslettersignups to anon;

create policy "public can sign up"
  on public.newslettersignups for insert
  to anon
  with check (true);
```

A hidden "website" field acts as a honeypot: if a bot fills it, the page shows
a success message but sends nothing.

## Deploying

GitHub Pages serves from the `main` branch root. Push to `main` and the site
updates automatically. In the repo settings, under **Pages**, set the source to
**Deploy from a branch**, branch `main`, folder `/ (root)`.
