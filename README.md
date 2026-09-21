# Anuvad — Translator

A simple, distraction-free web translator: type text, pick source/target languages, get a live translation with copy and text-to-speech.

**Live demo:** enable GitHub Pages (steps below) and it'll be at
`https://<your-username>.github.io/<repo-name>/`

## How it works

- Pure HTML/CSS/JS, no build step, no backend.
- Translation is powered by [MyMemory Translated](https://mymemory.translated.net/doc/spec.php) — a free, no-API-key translation API that works directly from the browser.
- Text-to-speech uses the browser's built-in `speechSynthesis` API (no external service).

## Deploying on GitHub Pages

1. Create a new repository on GitHub (public, so Pages works on the free tier).
2. Upload `index.html` to the repo (drag-and-drop on the GitHub web UI, or `git push` — see below).
3. Go to **Settings → Pages** in the repo.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**.
5. Pick the `main` branch and `/ (root)` folder, then **Save**.
6. Wait 1–2 minutes, then visit the URL GitHub shows on that same Pages settings page.

## Pushing with git instead of the web UI

```bash
git init
git add index.html README.md
git commit -m "Add translator app"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

## Limits of the free tier (MyMemory)

- Anonymous requests are capped at roughly **500 characters per request** — longer text is automatically split into chunks and translated piece by piece, so it still works, just in multiple calls.
- There's a **daily quota** for anonymous use (around 5,000 words/day at time of writing, shared across everyone using MyMemory anonymously from your network). If you hit it, translations will show a quota message until it resets.
- To raise the quota, MyMemory lets you append `&de=your@email.com` to the API request — see their [docs](https://mymemory.translated.net/doc/spec.php). This is optional and not wired in by default.

## Upgrading to a paid translation API

If you outgrow the free tier, swapping in Google Cloud Translation or Microsoft Translator is straightforward — replace the `translateChunk()` function in `index.html` with a call to their REST endpoint.

**Important:** GitHub Pages is a static host with no server, so any API key you put in the JavaScript is visible to anyone who views your page source. For a personal/portfolio project that's usually an acceptable risk with a key that has spending limits set; for anything public-facing at scale, you'd want a small backend (e.g. a Cloudflare Worker or Vercel serverless function) to hold the key instead.
