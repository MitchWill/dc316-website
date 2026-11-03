# DC316 Website — How It Works (the nerdy details)

Everything you need to run this site without Scout. No frameworks, no
databases, no build step. If you can edit a text file and use git, you
can run this site.

## 1. What this site actually is

A **static website**: a handful of plain files that a web server hands
to visitors exactly as written.

- `index.html`, `meetings.html`, `presentations.html`,
  `announcements.html`, `about.html` — the pages. HTML is just text
  with tags: `<h2>` is a heading, `<p>` is a paragraph, `<a href>` is
  a link. Open any of them in a text editor and you'll see it's
  readable.
- `styles.css` — one stylesheet that controls the look (colors, fonts,
  spacing) for every page. Change a color here, all pages change.
- That's it. There is no server-side code (no PHP, no Python, no
  database), which is why hosting is free and there's almost nothing
  that can break or be hacked.

## 2. How GitHub Pages serves it

GitHub Pages is free static hosting built into GitHub:

1. The files live in a git repository (a folder with version history).
2. When you push a change to the repository, GitHub copies the files
   to its web servers. Changes are usually live within a minute.
3. Visitors get the files over HTTPS automatically — GitHub provides
   the certificate, you don't buy or configure one.

Important consequences of "static":

- A form on the site can't send email by itself, because there's no
  server to receive it. That's why the contact form posts to
  **Formspree** (a free third-party service that receives the form and
  emails it to you). You create a form at formspree.io, get an ID like
  `abcd1234`, and put it in `about.html` where it says
  `YOUR_FORMSPREE_FORM_ID`.
- There are no logins, no comments, no dynamic content. Announcements
  are just HTML you edit by hand (see section 5).

## 3. Getting it live, step by step

1. **Create the repository.** On GitHub, create a new **public**
   repository named `dc316.github.io` (a project repo under your MitchWill
   account — the name is cosmetic; the custom domain setup works the same).
   (GitHub Pages on a private repo requires a paid plan; public is free.)
2. **Push these files.** From the folder containing them:
   ```
   git init
   git add .
   git commit -m "DC316 website v1"
   git branch -M main
   git remote add origin https://github.com/MitchWill/dc316.github.io.git
   git push -u origin main
   ```
3. **Turn Pages on.** Repo → Settings → Pages → "Deploy from a
   branch" → branch `main`, folder `/ (root)` → Save.
4. **Your URL** will be `https://mitchwill.github.io/dc316.github.io/`
   until the custom domain is connected (step 5 covers it).
5. **Before publishing, fill in the TODOs:**
   - `YOUR_DISCORD_INVITE_URL` in `index.html` and `about.html` — the
     Discord invite link. (Decide whether you want it public on the
     site; it currently goes nowhere on purpose.)
   - `YOUR_FORMSPREE_FORM_ID` in `about.html` — see section 2.
   - The deck download link in `presentations.html` — drop the deck
     file into a `presentations/` folder in the repo and point the
     link at it.

## 4. Custom domain (optional, later)

If you buy a domain (e.g. `dc316.org`, ~$10–15/year at Cloudflare or
Namecheap):

1. Add a file named `CNAME` to the repo containing just the domain
   name, e.g. `dc316.org`. (The repo already ships with a `CNAME` file
   containing `dc316.org`, so this step is done.)
2. At your registrar, point the domain at GitHub Pages: either four A
   records to GitHub's Pages IPs (185.199.108.153, .109.153, .110.153,
   .111.153) or a CNAME to `<your-username>.github.io`.
3. In repo Settings → Pages, check "Enforce HTTPS" once the domain
   resolves (GitHub issues the certificate automatically).

## 5. Everyday edits (the 95% case)

- **New announcement:** open `announcements.html`, copy a whole
  `<section class="card">...</section>` block, paste it above the older
  ones, change the date and text, commit, push. Also update the
  "Latest announcements" list on `index.html` if you want it mirrored
  there.
- **New presentation:** drop the file into a `presentations/` folder,
  add a card to `presentations.html` linking to it.
- **Next meetup changes:** edit the "Next meetup" card in `index.html`
  and the matching card in `meetings.html`.
- **Look and feel:** edit `styles.css`. The colors live in the `:root`
  block at the top (`--bg`, `--accent`, etc.).
- **New page:** copy an existing `.html` file, change the content, and
  add a link to it in the `<nav>` block on every page (the nav is
  copy-pasted per page — there is no shared template, which is the
  trade-off of having no build step).

To publish any edit: `git add .`, `git commit -m "what changed"`,
`git push`. That's the whole deployment pipeline.

## 6. If Scout is unavailable

You need exactly three things, all independent of Scout:

1. **This folder** (the site source) — keep a copy, e.g. in the git
   repo itself, which is the backup.
2. **Your GitHub account** — username, password, and 2FA method.
   Account recovery goes through GitHub, not through Scout.
3. **Your domain registrar account** (only if you add a custom domain)
   — same deal: registrar login, not Scout.

Nothing about the site depends on a paid service, a private server, or
any account Scout controls. The Formspree account (free tier) is the
only third party, and the form is the only thing that breaks if it
goes away — the rest of the site keeps working.

## 7. Costs, honestly

- GitHub Pages hosting: $0.
- Formspree free tier: $0 (50 submissions/month, plenty for this).
- Domain (optional): ~$10–15/year.
- Total to run: $0–15/year.

## 8. What would make you outgrow this setup

- Wanting other members to post without using git → add a CMS
  (Netlify CMS/Decap plugs into the same files) or move to WordPress.
- Wanting a working form without a third party, logins, or anything
  dynamic → move to Netlify (built-in forms + serverless functions).
- Either move reuses these same HTML files, so nothing here is wasted.
