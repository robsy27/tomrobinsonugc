# Tom Robinson — UGC Portfolio

One self-contained page. All content lives in the `PROFILE` object near the top of the
`<script>` block in `index.html`. Nothing else needs touching to update the site.

```
index.html          the whole site
assets/img/         hero-run.jpg, hero-golf.jpg, hero-travel.jpg   (hero panels + video posters)
                    portrait-01/02/03.jpg                          (the three portraits)
assets/video/       hero-golf.mp4, hero-travel.mp4                 (hero panels)
assets/logos/       brand logos (png or svg, transparent background)
originals/          untouched camera files, git-ignored
```

---

## How to update this

Open `index.html`, find `const PROFILE = {`, and edit. Save, refresh, done.

### Add a video
Find the right block in `work:` — `running`, `travel` or `golf` — and fill in a card:

```js
{ title: "5am tempo, one take",
  brand: "Nike",
  tag:   "Hook",                        // Hook / Testimonial / Unboxing / Voiceover / B-roll
  video: "assets/video/run-01.mp4",     // drop the mp4 in assets/video/
  embed: "",                            // use instead of video for a TikTok/YouTube link
  poster:"assets/img/run-01.jpg",       // still frame, shown before it plays
  alt:   "Tempo run at sunrise" }
```

Leave `video`, `embed` and `poster` empty and the card shows the designed "coming soon"
state instead — it never looks broken. Add a fourth card and the grid absorbs it.

### Add a brand logo
In `brands.items`, set `logo` to the file and `url` to the video you made for them:

```js
{ name: "Nike", mono: "NK", logo: "assets/logos/nike.svg", url: "https://..." }
```

`mono` is the two-letter fallback shown while there's no logo file.

### Change a price
In `rates.packages`, edit `from`. Add-ons live in `rates.addons`, and the
"what's in one UGC video" list is `rates.included`. Prices are plain strings — write
`"£150"`, `"from £400"`, anything you like.

They currently read `"£—"` and `"TBC"` as deliberate placeholders. Put a real number in
`from` and the small "FROM" label appears above it on its own; any value without a digit
is treated as a placeholder and the label stays hidden.

### Change a stat
Two places: `about.stats` (the follower strip) and `results.stats` (the analytics cards).
Each is `{ v, l, n }` — value, label, note. The value animates as a counter automatically,
so `"3"`, `"5–7"` and `"7.8%"` all work. A value with no digits, like `"—"`, is left alone.

### Swap a portrait
Three slots, three pictures. Change the `src` in `photos`:

```js
hero:    { src: "assets/img/portrait-01.jpg", ... }   // the card beside your name
rail:    { src: "assets/img/portrait-02.jpg", ... }   // the desktop sidebar thumbnail
closing: { src: "assets/img/portrait-03.jpg", ... }   // "Let's create together"
```

The duotone, grain and gradient treatment is CSS, so any photo picks it up. Hero wants
roughly 3:4, the other two roughly 4:5. Always update the `alt` text to match the picture.

### Change the hero background
The three panels behind your name live in `heroMedia`. Each is either an image or a
looping video, and they always split the width equally:

```js
{ kind: "video", label: "Golf", src: "assets/video/hero-golf.mp4",
  poster: "assets/img/hero-golf.jpg", alt: "..." }
```

Keep clips short (2-4 seconds), vertical, and under about 2MB — they are heavily darkened
behind the type, so quality matters far less than file size. The poster is what shows
before the video loads and on phones that block autoplay. Videos pause when the hero
scrolls out of view and never play at all for visitors who ask for reduced motion.

Camera files go through two steps before they belong in `assets/`:

```bash
avconvert --preset Preset960x540 --source clip.mov --output assets/video/clip.mp4
qlmanage -t -s 1000 -o . clip.mov    # then save the png as the poster jpg
```

### Keep the claims honest
There are no invented client numbers anywhere in the config. The Results tiles show `"—"`
until there is something real to put in them, and the brand wall is six empty slots.
When a campaign lands, fill in the tile value and the note that sits under it.

---

## Put it on GitHub

```bash
cd "/Users/tomrobinson/UGC Portfolio"
git init
git add .
git commit -m "UGC portfolio"
git branch -M main
```

Create an empty repo at github.com/new (no README, no .gitignore), then:

```bash
git remote add origin https://github.com/YOUR-USERNAME/ugc-portfolio.git
git push -u origin main
```

To update later: `git add . && git commit -m "update" && git push`

---

## Connect it to your Hostinger domain

**Option A — Hostinger's GitHub deploy (recommended, auto-updates on push)**

1. hPanel → **Websites** → your domain → **Advanced** → **GIT**
2. Repository: `https://github.com/YOUR-USERNAME/ugc-portfolio.git`
   Branch: `main`   Directory: leave blank (deploys to `public_html`)
3. **Create** → then **Deploy**
4. Open the **Auto Deployment** option and copy the webhook URL
5. In GitHub: repo → **Settings** → **Webhooks** → **Add webhook** → paste the URL,
   content type `application/json`, event: just the push event → **Add webhook**

Every `git push` now redeploys the site.

**Option B — straight upload**

hPanel → **File Manager** → `public_html` → upload `index.html` and the `assets` folder.

**Then, either way**

- hPanel → **Websites** → **SSL** → install the free certificate, and turn on
  "Force HTTPS" once it's issued.
- In `index.html`, update `<meta property="og:url">` to your real domain and drop a
  1200×630 image at `assets/img/og-cover.jpg` so link previews look right.
- Add `assets/img/apple-touch-icon.png` (180×180) and uncomment that `<link>` in the head.
