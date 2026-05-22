# Kick Out the Jams

A mobile-first website for browsing a vinyl record collection. Guests scan a QR code, explore the catalog by genre, search by artist or vibe, and request records via push notification.

**Live site:** `https://yourusername.github.io/record-collection/`

## How It Works

The site loads a CSV file (`records.csv`) at runtime and renders the full collection — no build step, no backend, no database. The CSV is the single source of truth.

### Features

- **Genre filtering** — tap a genre chip to filter; tap again to clear
- **Search** — filters by album title, artist, or "if you like" description
- **Shuffle** — "Pick one for me" selects a random record
- **Album detail modal** — tap any record to see release year, acquisition date, genre tags, and full track listing with favorited tracks
- **Request notifications** — guests can tap "Request" in the modal to send a push notification to the owner's phone via [ntfy.sh](https://ntfy.sh)

## Project Structure

```
index.html      ← page structure + JavaScript logic
styles.css      ← all styling
records.csv     ← collection data (the only file you update regularly)
```

## Updating Your Collection

1. Edit your collection in Notion (or any spreadsheet)
2. Export as CSV
3. Rename the file to `records.csv`
4. Upload to this repo, replacing the old file
5. The live site reflects the changes immediately

### CSV Columns

| Column | Required | Example |
|---|---|---|
| `Album` | Yes | The Rise and Fall of a Midwest Princess |
| `Artist` | Yes | Chappell Roan |
| `Genre Section` | Yes | 12" |
| `Great if you like:` | No | upbeat, 80s-inspired synthpop |
| `Released` | No | 2023 |
| `Acquired` | No | 2024 |
| `Genres` | No | Pop, Alt Pop, Dance Pop, Synthpop |
| `Art` | No | https://url-to-cover-image.jpg |
| `Tracks` | No | See format below |

### Genre Section Values

The `Genre Section` column maps to the display labels on the site:

| CSV Value | Displays As |
|---|---|
| `12"` | 💿 12" (Pop) |
| `Electronic` | 🎛️ Electronic |
| `Funk/Soul` | 🪩 Funk/Soul |
| `Indie` | ☕️ Indie |
| `International` | 🌎 International |
| `Jazz` | 🎷 Jazz |
| `Rock` | 🎸 Rock |
| `OST` | 🎮 OST |

### Track Listing Format

Tracks go in a single cell using this format:

```
A: Track One*, Track Two, Track Three | B: Track Four, Track Five*
```

- Sides are separated by ` | `
- Tracks within a side are separated by `,`
- Append `*` to mark a track as a favorite (shows a ☆ in the modal)
- Single-letter side labels (`A`, `B`, `C`) are automatically expanded to `A-Side`, `B-Side`, etc.

Example with multiple sides:
```
A: Do I Wanna Know?*, R U Mine?* | B: Mad Sounds, Knee Socks*, I Wanna Be Yours*
```

## Notifications Setup

The "Request" button uses [ntfy.sh](https://ntfy.sh) for free push notifications with no account required.

1. Install the **ntfy** app ([iOS](https://apps.apple.com/us/app/ntfy/id1625396347) / [Android](https://play.google.com/store/apps/details?id=io.heckel.ntfy))
2. Subscribe to your topic in the app
3. Set `NTFY_TOPIC` in `index.html` to match your topic name

The topic name acts as a password — keep it unique and don't share it publicly.

## Fonts

The site uses **New Spirit** (serif, via Adobe Fonts) and **Courier New** (monospace, system font). The Adobe Fonts embed is loaded from Typekit:

```html
<link rel="stylesheet" href="https://use.typekit.net/hhk3ysu.css">
```

To use your own Adobe Fonts project, replace the Typekit URL and add your GitHub Pages domain to the project's allowed domains at [fonts.adobe.com](https://fonts.adobe.com).

## Album Art

Add an `Art` column to your CSV with direct image URLs for each album. Discogs is the easiest source: go to the release page, right-click the cover image, and copy the image URL.

Records without art show a colored letter placeholder.

## Local Development

No build tools needed. Open `index.html` in a browser — but note that the CSV fetch requires a local server due to CORS. The simplest option:

```bash
npx serve .
```

Then visit `http://localhost:3000`.

## Deployment

This is a static site. Upload all three files to the root of a GitHub repository, enable GitHub Pages in Settings → Pages → Deploy from branch (`main`, root), and the site will be live at `https://yourusername.github.io/repo-name/`.
