# Deployment Guide — tejagollapudi.com

## Overview
Host the site on **GitHub Pages** (free) and route your **Cloudflare** domain to it.

---

## Step 1 — Create the GitHub repo

1. Go to https://github.com/new
2. Name the repo exactly: `tejagollapudi.github.io`
   - This is the special GitHub Pages repo name for your account
   - Visibility: **Public**
3. Click **Create repository** (do NOT initialize with README)

---

## Step 2 — Push the site files

Open **PowerShell** or **Git Bash**, navigate to this folder, then run:

```powershell
cd "C:\Users\Teja\projects\tejagollapudi-website"

git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/tejagollapudi/tejagollapudi.github.io.git
git push -u origin main
```

---

## Step 3 — Enable GitHub Pages

1. Go to your new repo on GitHub
2. Click **Settings** → **Pages** (left sidebar)
3. Under **Source**, select:
   - Branch: `main`
   - Folder: `/ (root)`
4. Click **Save**
5. Under **Custom domain**, type: `tejagollapudi.com` → click **Save**
6. Check **Enforce HTTPS** (after DNS propagates, usually < 1 hour)

GitHub will auto-detect the `CNAME` file — this is normal.

---

## Step 4 — Configure Cloudflare DNS

Log into **Cloudflare → tejagollapudi.com → DNS → Records**

Add these **4 A records** (GitHub Pages IPs):

| Type | Name | Content         | Proxy |
|------|------|-----------------|-------|
| A    | @    | 185.199.108.153 | DNS only (grey cloud) |
| A    | @    | 185.199.109.153 | DNS only |
| A    | @    | 185.199.110.153 | DNS only |
| A    | @    | 185.199.111.153 | DNS only |

Add this **CNAME record** for `www`:

| Type  | Name | Content                         | Proxy |
|-------|------|---------------------------------|-------|
| CNAME | www  | tejagollapudi.github.io         | DNS only |

> **Important:** Set Proxy to **DNS only** (grey cloud), NOT orange cloud.
> GitHub Pages handles HTTPS itself — Cloudflare proxy interferes with certificate issuance.

---

## Step 5 — Wait for propagation

- DNS typically propagates in **5–30 minutes**
- HTTPS certificate is issued automatically by GitHub (Let's Encrypt)
- Full propagation can take up to 24 hours in rare cases

Check status at: https://github.com/tejagollapudi/tejagollapudi.github.io/settings/pages

---

## Updating the site later

Edit `index.html`, then:

```powershell
git add index.html
git commit -m "Update content"
git push
```

GitHub Pages auto-deploys within ~30 seconds.

---

## TODO — Fill in your real content

Search for `[` in `index.html` to find all placeholders:

- `[Company]` — your employer
- `[YOUR-EMAIL]` — contact email
- `[YOUR-LINKEDIN]` — LinkedIn slug (e.g. `teja-gollapudi`)
- `[YOUR-SCHOLAR-ID]` — Google Scholar user ID
- `[N]+` — real numbers for publications, citations, etc.
- `[Paper Title N]` — your actual paper titles, venues, links
- `[Publication N]` — press outlet names and article links
- Headshot: replace the `.photo-placeholder` div with `<img src="photo.jpg" ...>`
- `resume.pdf` — add your CV file to the repo root
