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

## Inspiration map

`inspiration.html` maps poems by originality, creativity and fluidity. It opens at `https://<your-username>.github.io/poetry-dashboard/inspiration.html` and uses the same sign-in as the statistics page; if you are already signed in there, it opens straight away.

- It loads every poem's text from `public.poems` (row-level security applies as above) and scores them in the browser each time it loads. Nothing is written back to the database.
- Each poem is scored only up to its last `---` line, so the date, place and notes after it don't count. Poems under 30 words or 4 lines are left out.
- Scores are percentiles within your own poems (50 = your average). Originality is how rare the words are across your archive, creativity is how few words repeat in the first 60, and fluidity is how often a line break falls mid-phrase.
- The page contains no poem data itself; titles, tags and scores appear only after signing in.

## Dragonfly timeline

`dragonfly.html` shows how the dragonfly notebook (Dec 2018 – Mar 2022) changed, quarter by quarter and across seven stretches. It opens at `https://<your-username>.github.io/poetry-dashboard/dragonfly.html` and uses the same sign-in as the other pages; if you are already signed in there, it opens straight away.

- Each time it loads, it calls the database function `public.dragonfly_timeline()`, which computes the numbers live from `public.poems`. There is no data file to update when poems sync.
- Only the owner's account and accounts listed in the database table `analysis.page_viewers` get data back; anyone else sees the sign-in box. Listed viewers are read-only: they have no access to the `poems` tables, so the other pages show them nothing and they can't change anything. The links to the other pages appear only for the owner.
- The sign-in box also takes a plain username such as `emily`. The page turns it into `emily@poetry.invalid`, a placeholder address that never receives mail, so a username account can't use emailed sign-in links or reset its own password; the owner sets its password in Supabase (Authentication → Users).
- The stretch descriptions are written text inside the page. The database checks each of their claims against the live numbers, and if one stops holding, a note under the header says which. Edit that sentence in the `ERAS` text near the bottom of `dragonfly.html`.
- The metrics, theme word lists, stretch dates, excerpts and claim checks live in the database's `analysis` schema.

## Notes

- Data refreshes each time the page loads; use **Refresh** to reload without signing in again. Your session is remembered in the browser until you click **Sign out**.
- Poems tagged `ignore`, `not-poem` or `not-poetry` are left out everywhere.
- To load quickly, the page reads poem details from the `poems_meta` view (no full text) and fetches a poem's full text only when you click its title. The view uses the same row-level security as `poems`.
- `themes.html` shows how often each theme tag appears in every quarter (share of that quarter's poems), with the poems behind each cell. It uses the same sign-in.
- Libraries load from cdn.jsdelivr.net (d3 7.9.0 and supabase-js 2.45.4).
