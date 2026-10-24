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

`focus` is where the panel crops from when it is narrow. A phone shows each panel at
roughly 125px wide, so `object-fit: cover` throws away most of the width — if your subject
sits left of centre, lower the first number (`"26% 40%"` keeps someone on the left in
frame). `"50% 50%"` is the plain centre. It barely matters on desktop, where the panels
are wide enough that almost nothing is cropped.

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

## Where it lives

| | |
|---|---|
| Live site | https://tomrobinsonugc.com |
| Repository | https://github.com/robsy27/tomrobinsonugc |
| Host | Hostinger shared hosting, served from `public_html` |
| SSL | Lifetime certificate, HTTPS forced (http:// 301s to https://) |

## Publishing a change

Auto-deployment is already connected: Hostinger watches the `main` branch of the
repository and redeploys the document root on every push. So:

```bash
cd "/Users/tomrobinson/UGC Portfolio"
git add -A
git commit -m "what you changed"
git push
```

Give it a few seconds, then hard-refresh the site (Cmd+Shift+R). Nothing to click in
hPanel. If a change doesn't show, check the push landed on GitHub first — the deploy only
ever mirrors what is on `main`.

Deployment settings live in hPanel under **Websites → tomrobinsonugc.com → Advanced →
GIT**, if you ever need to point it at a different branch or switch it off.

## Images and video

Anything in `assets/` is committed to the repository and deployed with the page, so keep
it lean — the two hero clips are about 1.9MB each, which is the sensible ceiling. The
`originals/` folder is git-ignored, so your untouched camera files stay on your Mac and
never reach the server.

## Before you send the link to a brand

- The prices read `£—` and `TBC` until you set them.
- The Results tiles read `—` until you have campaigns to report.
- The brand wall is six empty slots.
- The nine video cards are placeholders; add real clips as you shoot them.
