# Lamada Smouha — Website

A one-page site for Lamada Smouha (fast food, Sidi Gaber, Alexandria) with a
live open/closed status and an editable menu board backed by Supabase.

## Files

- `index.html` — the whole site (structure, styles, and logic).
- `config.js` — your Supabase project URL and anon key. Not committed with
  real secrets by default — fill in your own values before deploying.

## 1. Add your Supabase credentials

Open `config.js` and replace the placeholders:

```js
window.SUPABASE_URL = "https://xxxx.supabase.co";
window.SUPABASE_ANON_KEY = "your-anon-public-key";
```

You'll find both under **Project Settings > API** in your Supabase dashboard.

Make sure you've already created, in Supabase:
- a `menu_items` table with columns: `name` (text), `price` (text),
  `category` (text), `image_url` (text), `created_at` (timestamptz, default `now()`)
- open read/write policies for the `anon` role on that table
- a **public** storage bucket named `menu-photos`

Without valid credentials, the site still works, but the menu only saves in
your current browser tab (a banner at the top will let you know).

## 2. Push to GitHub

```bash
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO.git
git push -u origin main
```

## 3. Turn on GitHub Pages

1. In your repo on GitHub, go to **Settings > Pages**.
2. Under "Build and deployment", set **Source** to "Deploy from a branch".
3. Choose the `main` branch and `/ (root)` folder, then **Save**.
4. GitHub gives you a live URL shortly after, like:
   `https://YOUR-USERNAME.github.io/YOUR-REPO/`

## Notes

- `config.js` holds the anon (public) Supabase key only — that key is
  designed to be exposed in frontend code. Never put a Supabase *service
  role* key here.
- Since there's no login system, anyone with the live link can edit the
  menu. That's fine for a small single-owner site, but worth knowing.
- If you'd rather keep `config.js` out of your public repo (e.g. to swap
  credentials per environment), add it to `.gitignore` and instead create it
  manually on your hosting provider, or inject the values at build time.
