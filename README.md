# Co-Crafted AI Academy — Deployment Guide

## What You're Deploying

A 16-slide interactive wireframe with a live AI assistant (slide 16) powered by Claude.
The AI assistant has the full CLAUDE_AI_ACADEMY_ARCHITECT knowledge document loaded
and will answer questions about the model — but will never speculate on financials,
headcount, enrollment numbers, or budgets.

---

## Step-by-Step: Deploy to Vercel (Free)

### 1. Get an Anthropic API Key

1. Go to https://console.anthropic.com
2. Create an account or sign in
3. Go to **API Keys** → **Create Key**
4. Copy the key (starts with `sk-ant-...`)
5. This key costs based on usage — roughly $0.01–0.03 per conversation turn

### 2. Push to GitHub

1. Create a new repository on GitHub (e.g., `cocrafted-academy`)
2. Upload the contents of this folder to the repo:

```
cocrafted-academy/
├── api/
│   └── chat.js          ← Serverless function (holds your API key securely)
├── public/
│   └── index.html       ← The full interactive site
├── src/
│   └── App.jsx          ← Source JSX (for reference/editing)
├── package.json
├── vercel.json
└── README.md
```

### 3. Deploy on Vercel

1. Go to https://vercel.com and sign in with GitHub
2. Click **"Add New Project"**
3. Import your `cocrafted-academy` repository
4. In **Settings → Environment Variables**, add:
   - **Name:** `ANTHROPIC_API_KEY`
   - **Value:** your `sk-ant-...` key
5. Click **Deploy**

That's it. Vercel gives you a URL like `cocrafted-academy.vercel.app`.

### 4. (Optional) Custom Domain

In Vercel dashboard → your project → **Settings → Domains**, add your custom domain.
Vercel handles SSL automatically.

---

## How It Works

```
[User's Browser]
       │
       │  POST /api/chat  (messages only, no API key)
       ▼
[Vercel Serverless Function: /api/chat.js]
       │
       │  Adds API key from environment variable
       │  Forwards to Anthropic API
       ▼
[Anthropic API]
       │
       │  Returns Claude's response
       ▼
[Back to browser → displayed in chat]
```

The API key **never** leaves the server. Users only see the chat interface.

---

## Cost Estimate

- **Vercel hosting:** Free tier covers most usage (100GB bandwidth/month)
- **Anthropic API:** ~$0.01–0.03 per conversation turn
  - 100 conversations/month ≈ $3–10
  - 1,000 conversations/month ≈ $30–100
- You can set spending limits in the Anthropic console

---

## Security Notes

- The API key is stored as a Vercel environment variable — never in code
- The serverless function acts as a proxy — users cannot see or access the key
- To restrict access to your domain only, uncomment the origin check in `api/chat.js`
- Consider adding rate limiting if you expect high traffic

---

## Editing the Content

- **Slides 1–15:** Edit `public/index.html` or `src/App.jsx` directly
- **AI Assistant knowledge:** The knowledge document is embedded in the JSX inside the `KNOWLEDGE` constant
- **AI behavior rules:** The `SYSTEM_PROMPT` constant controls what the assistant will and won't do
- After editing, push to GitHub → Vercel auto-deploys

---

©2026 Angie Carel Generative AI Advisory
