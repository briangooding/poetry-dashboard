# Poetry statistics dashboard

A single-page dashboard of poetry stats (when, where and what poems are about) that reads live from the Supabase `poems` table.

## How the password works

The page asks for the **email and password of your Supabase account** for the poetry project. It signs in with Supabase Auth and loads the poems with your session, so the database's row-level security decides who can read them. The existing rule only lets the owner's account read `public.poems`, so anyone else who opens the page sees nothing, even if they find the URL or the code.

The only key in `index.html` is the project's *publishable* key, which is designed to be public. No secret keys are stored in this repository.

## Setup

1. Create a new repository on GitHub (for example `poetry-dashboard`).
2. Upload `index.html` and this `README.md` (Add file → Upload files → Commit).
3. To open it as a website, go to **Settings → Pages**, set Source to *Deploy from a branch*, choose `main` and `/ (root)`, and save. The site appears at `https://<your-username>.github.io/poetry-dashboard/` after a minute.
   - This repository is public so GitHub Pages works on the free plan. Only the page code is visible; the poems load only after signing in.
4. Open the page and sign in with your Supabase email and password.

### Signing in without a password

Signing in to supabase.com with Google is a separate login. This page uses the poetry project's own user (your email), which has its own password. If you don't know it:

1. In the Supabase dashboard, open the poetry project → **Authentication → URL Configuration** and add the page's address (for example `https://<your-username>.github.io/poetry-dashboard/`) under **Redirect URLs**. Do this once.
2. Open the page from that address, type your email, and click **Email me a sign-in link** (or **Forgot password?** to set a new password).
3. Open the emailed link in the same browser. It brings you straight into the dashboard (or to a box to choose your new password).

Email links only work when the page is opened from its web address, not as a file on your computer.

## Notes

- Data refreshes each time the page loads; use **Refresh** to reload without signing in again. Your session is remembered in the browser until you click **Sign out**.
- Poems tagged `ignore`, `not-poem` or `not-poetry` are left out everywhere.
- To load quickly, the page reads poem details from the `poems_meta` view (no full text) and fetches a poem's full text only when you click its title. The view uses the same row-level security as `poems`.
- `themes.html` shows how often each theme tag appears in every quarter (share of that quarter's poems), with the poems behind each cell. It uses the same sign-in.
- Libraries load from cdn.jsdelivr.net (d3 7.9.0 and supabase-js 2.45.4).
