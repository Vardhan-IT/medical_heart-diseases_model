# Deploying CORDIS with a working AI assistant

The assistant needs your Anthropic API key on a server, never in the browser.
This folder is already set up for Vercel, which gives you that server for free.

## Folder structure (keep this exact layout in your GitHub repo)

```
your-repo/
├── index.html        ← the app (was cordis.html)
└── api/
    └── chat.js        ← serverless function that holds your API key
```

## Steps

1. **Get an Anthropic API key**
   Go to https://console.anthropic.com → Settings → API Keys → Create Key.
   Copy it (you won't be able to see it again).

2. **Push this folder to GitHub**
   ```
   git init
   git add .
   git commit -m "CORDIS with working AI assistant"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
   git push -u origin main
   ```

3. **Import the repo into Vercel**
   - Go to https://vercel.com → New Project → Import your GitHub repo.
   - Framework preset: choose "Other" (no build step needed — it's static + one function).
   - Before clicking Deploy, open **Environment Variables** and add:
     - Name: `ANTHROPIC_API_KEY`
     - Value: (paste the key from step 1)
   - Click **Deploy**.

4. **Done.** Vercel gives you a URL like `your-project.vercel.app`. Open it —
   the intake form, risk gauge, and doctor summary all work exactly as before,
   and the AI assistant now works too, because `/api/chat` runs on Vercel's
   server with your key, and the browser only ever talks to `/api/chat`.

## If you still want it on GitHub Pages too

GitHub Pages can host the static form/gauge/summary (everything except the AI
assistant, since Pages can't run server code). If you want both live, you can
publish the same `index.html` to GitHub Pages for the calculator, and point
people to the Vercel URL for the full version with the assistant — or just
use the Vercel URL as your only live link, since it hosts everything.

## Costs

Vercel's free tier covers this easily (one lightweight function call per chat
message). You do pay Anthropic per API call at standard token rates — keep an
eye on usage in the Anthropic console if you expect a lot of traffic.

## Troubleshooting

- **"Assistant error: ... ANTHROPIC_API_KEY"** → the env variable isn't set,
  or you need to redeploy after adding it (Vercel → Project → Settings →
  Environment Variables → then Deployments → Redeploy).
- **Chat spins forever / network error** → check the browser console (F12);
  confirm requests are going to `/api/chat`, not `api.anthropic.com`.
