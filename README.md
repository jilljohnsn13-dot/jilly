# Lease Redline

A website where anyone can upload a Letter of Intent (LOI) or commercial lease and get:

- a term-by-term review against your negotiation playbook (missing / negotiate / looks good)
- a list of LOI terms the lease changed or left out
- a ready-to-send email to the property manager

Visitors don't need a Claude account. Reviews run on **your** Anthropic API key, so you pay for each review.

## What's in this folder

| File | What it does |
|---|---|
| `index.html` | The website people see |
| `lib/playbook.js` | **Your negotiation rules.** Edit targets here; the site and the AI both use it |
| `netlify/edge-functions/review.js` | Sends the documents to Claude and streams the review back (your API key stays hidden here) |
| `netlify.toml`, `package.json` | Settings for Netlify |

## Put it online (about 15 minutes, free hosting)

### 1. Get an Anthropic API key
1. Go to **console.anthropic.com** and create an account.
2. Under **Billing**, add credits. Then set a **monthly spend limit** so a busy month can't surprise you.
3. Under **API keys**, create a key. Copy it somewhere safe; you'll paste it in step 3.

### 2. Put the files on GitHub
Netlify's drag-and-drop upload only publishes plain pages, not the review step, so the files go through GitHub.
1. Create a free account at **github.com**.
2. Click **New repository**, name it `lease-redline`, and create it.
3. Click **uploading an existing file**, then drag in everything from this folder (`index.html`, `README.md`, `package.json`, `netlify.toml`, and the `lib` and `netlify` folders).
4. Click **Commit changes**.

### 3. Publish it on Netlify
1. Sign in at **app.netlify.com** (you can use your GitHub account).
2. Click **Add new site → Import an existing project → GitHub**, and pick `lease-redline`.
3. Leave the build settings as they are and click **Deploy**.
4. Go to **Site configuration → Environment variables** and add:
   - `ANTHROPIC_API_KEY` = the key from step 1
   - *(optional)* `ACCESS_CODE` = a code people must type to run a review, if you only want your team using it
5. Go to **Deploys → Trigger deploy → Deploy site** so the key takes effect.
6. Your link looks like `something.netlify.app`. Rename it under **Site configuration → Change site name**, or add your own domain under **Domain management**.

## Changing things later
- **Negotiation targets:** edit `lib/playbook.js` on GitHub. Netlify republishes automatically.
- **Model:** add an environment variable `CLAUDE_MODEL` (default `claude-sonnet-5-5`).
- **Limits:** each visitor can run about 8 reviews an hour; scanned PDFs read up to 5 pages. Text PDFs and Word files work up to about 150,000 characters each.

## If something goes wrong
- *"The site isn't connected to Claude yet"*: the `ANTHROPIC_API_KEY` variable is missing. Add it, then trigger a new deploy.
- *Long leases stop partway*: try the review again. If it keeps happening, upload the LOI alone or paste only the main lease sections.
- *Reviews say the service is busy*: check your credit balance and spend limit at console.anthropic.com.

This is general business guidance, not legal advice.
