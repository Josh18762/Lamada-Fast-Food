# Lamada Smouha — Website

A one-page site for Lamada Smouha (fast food, Sidi Gaber, Alexandria) with a
live open/closed status and an editable menu board backed by Supabase.

## Files

- `index.html` — the whole site (structure, styles, and logic).
- `config.js` — your Supabase project URL and anon key. This is safe to
  commit as-is since it only holds the public anon key (see note below) —
  just fill in your real values before deploying.
- `.gitignore` — keeps OS/editor junk files out of the repo.

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
- a **public** storage bucket named `menu-photos`

Without valid credentials, the site still works, but the menu only saves in
your current browser tab (a banner at the top will let you know).

## Owner-only editing

Only someone logged in as the owner can add photos, add items, change
prices, or change an item's category. Everyone else sees a plain, read-only
menu with no edit controls. To set this up:

### 1. Create your owner account

In Supabase, go to **Authentication > Users > Add user**, and create a
user with your email and a password. This is the only account that should
exist — the site has no public sign-up form, so nobody else can create one
through the website itself.

To be extra safe, also turn off self sign-up: **Authentication > Providers
> Email**, and disable "Allow new users to sign up."

### 2. Lock down the table with RLS policies

Go to **Authentication > Policies** for `menu_items` and set:

- **SELECT** — allow for the `anon` role (so everyone can view the menu)
- **INSERT / UPDATE / DELETE** — allow only for the `authenticated` role
  (so only someone logged in can edit)

Do the same for the `menu-photos` storage bucket: public read, but
uploads restricted to `authenticated`.

This is the part that actually enforces owner-only editing — hiding the
edit buttons in the page is just a nicer experience for visitors, not the
security boundary. The database policies are what stop a stranger from
editing the menu directly.

### 3. Log in on the live site

On the deployed site, click **Owner login** near the menu, sign in with the
account from step 1, and the add/edit/delete/photo controls will appear.
They stay hidden for every other visitor.

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
