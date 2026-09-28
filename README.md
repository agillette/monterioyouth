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

## Deploying

GitHub Pages serves from the `main` branch root. Push to `main` and the site
updates automatically. In the repo settings, under **Pages**, set the source to
**Deploy from a branch**, branch `main`, folder `/ (root)`.
