# Protektor Warehouse — deployment guide

This is a plain static site: one `index.html`, no build step, no dependencies
to install. That makes it simple to deploy, but a few things below matter
specifically for GitHub + Vercel.

## What's in this folder

```
index.html
hall-archival-1.jpg
hall-archival-2.jpg
sprites/
  sprite_01.jpg … sprite_05.jpg
```

8 files total. The scrolling video is **not** 300 separate frame images —
it's 150 frames packed into 5 sprite sheet images (each a 6×5 grid), which
`index.html` slices apart in the browser with canvas. This is what keeps the
upload small and under GitHub's ~100-files-per-drag-and-drop limit, while
still playing back all 150 frames (the script crossfades between adjacent
frames on scroll, so it reads as smooth even at half the original frame
count).

**Do not** try to "improve" this by re-exporting to individual frame files —
that's the exact problem this structure avoids.

## 1. Push it to GitHub

**Easiest (no git installed):**
1. Create a new repository at github.com → "New repository"
2. On the empty repo page, click **"uploading an existing file"**
3. Drag in all 8 files **keeping the `sprites` folder structure** — GitHub's
   web uploader preserves folder paths if you drag the whole `sprites` folder
   in, not just the files inside it
4. Commit

**With git:**
```bash
git init
git add .
git commit -m "Protektor Warehouse site"
git branch -M main
git remote add origin https://github.com/<you>/<repo>.git
git push -u origin main
```

## 2. Deploy on Vercel

1. [vercel.com](https://vercel.com) → **Add New → Project**
2. Import the GitHub repo you just created
3. Framework Preset: choose **"Other"** (this is a plain static site, not a
   framework project — Vercel doesn't need to build anything)
4. Build Command: leave **blank**
5. Output Directory: leave as **`./`** (default)
6. Deploy

That's it — no environment variables, no build step. Vercel will serve
`index.html` and the two asset folders exactly as they are.

## Before going properly live

- Swap the placeholder contact details (`blank@blank.com`, `+1234567890`)
  for the real ones — search for both strings in `index.html`, each appears
  twice (footer + finale CTA)
- Every internal link (nav, footer, CTA) uses relative paths, so nothing
  needs to change for the move from local testing to a real domain
