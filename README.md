# Dragonfly notebook timeline

`index.html` is the whole page. Drop this `dragonfly/` folder into the repo that serves your other poetry pages.

## How it works

- **Sign-in.** The page signs in to the Supabase "poetry" project with your email account, using a password or an emailed link.
  - Supabase keeps the session per site. If your poetry statistics and inspiration pages are on the same GitHub Pages site and you're signed in there, this page opens straight away.
- **Live numbers.** Every time the page loads, it calls `public.dragonfly_timeline()` in the database, which returns current numbers straight from `public.poems`.
  - There's no data file and no scheduled job. Nothing needs updating when your poems sync.
- **Access.** The function only returns data to a session signed in as briangoodingth@gmail.com. Anyone else, or anyone signed out, gets only the sign-in box. No poem text sits in the repo.
- **Checks.** The page tests its 22 written claims and its excerpts against the live data. If any no longer hold, a note under the header lists them. To fix one:
  - edit that sentence in the `ERAS` text in `index.html`, or
  - ask Claude to run the redo-poetry-analysis skill.

## If you sign in with an emailed link

In Supabase, go to Authentication, then URL Configuration, and add the page's address to Redirect URLs, for example `https://<you>.github.io/<repo>/dragonfly/`. Password sign-in doesn't need this.
