# 漫画コレクション PWA

A Progressive Web App designed for serious Japanese manga collectors to track their collection while shopping in bookstores, with full offline functionality.

## Features

### Core Functionality
- **Offline-First Architecture**: Works completely offline using IndexedDB for local storage
- **Automatic Sync**: Syncs with Airtable when connection is available
- **Optimized In-Store Experience**: 
  - Quick volume lookup filtered by want/pending status
  - Full-screen cover images for easy matching with physical books
  - One-tap status updates (mark as purchased while shopping)
  - Grouped by publisher → imprint → creator for easy browsing

### Data Management
- Displays volumes from your Airtable database
- Shows cover images, imprint logotypes, release dates
- Filter by status: want, pending, queued, purchased, shelved
- Search by title or creator name (Japanese)
- Updates queue offline and sync when back online

### Technical Features
- Progressive Web App (installable to iOS home screen)
- IndexedDB for offline storage
- Service worker for offline functionality
- Optimized for iPhone (but works on any mobile device)
- Dark theme optimized for low-light bookstore browsing

## Setup Instructions

### 1. Upload Files to Web Hosting

You need to host these files on a web server with HTTPS (required for PWAs):

**Files to upload:**
- `manga-tracker.html` (the main app)
- `manifest.json` (PWA configuration)

**Hosting Options:**
- GitHub Pages (free, easy)
- Netlify (free tier available)
- Vercel (free tier available)
- Your own web hosting

### 2. GitHub Pages Setup (Recommended)

1. Create a new GitHub repository
2. Upload both files to the repository
3. Go to Settings → Pages
4. Select "main" branch as source
5. Your app will be available at: `https://[username].github.io/[repo-name]/manga-tracker.html`

### 3. Update API Token (Important)

**Security Note:** The current setup includes your API token directly in the HTML file. For production use, you should:

1. **Option A - Netlify Functions** (Recommended):
   - Create a serverless function to proxy Airtable requests
   - Store the API token as an environment variable
   - Update the fetch calls to use your function endpoint

2. **Option B - Keep as-is** (Simpler but less secure):
   - Only if you're comfortable with the token being visible in source code
   - Consider using a read-only token with limited scopes
   - Regularly rotate your API token

### 4. Install on iPhone

Once hosted:

1. Open the URL in Safari on your iPhone
2. Tap the Share button (square with arrow)
3. Scroll down and tap "Add to Home Screen"
4. Name it "漫画コレクション" or whatever you prefer
5. Tap "Add"

The app will now appear on your home screen like a native app!

### 5. First Use

1. Launch the app from your home screen
2. Wait for initial data load (requires internet)
3. After first load, all data is cached offline
4. The app will sync automatically when you have internet

## Using the App in Bookstores

### Quick Workflow

1. **Before shopping**: Open the app at home to ensure latest data is synced
2. **In store**: App works completely offline
   - Filter shows "want" and "pending" by default
   - Volumes grouped by publisher and imprint
   - Tap any volume to see full cover image
   - Scroll down to see imprint logotype
3. **Found a volume**: Tap "ステータス変更" → select "purchased"
4. **After shopping**: When back online, changes sync automatically to Airtable

### Filter Tags

- **欲しい & 予約中** (Want & Pending) - Default view for shopping
- **待機中** (Queued) - Volumes waiting to be purchased
- **購入済み** (Purchased) - Recently purchased volumes
- **本棚** (Shelved) - Volumes already on your shelf
- **全て** (All) - Show everything

### Search

Type in Japanese (hiragana, katakana, or kanji) to search by:
- Title (タイトル)
- Creator reading (漫画家の音読み)

## Offline Behavior

### What Works Offline
- ✅ View all cached volumes
- ✅ Search and filter
- ✅ Change status
- ✅ View cover images and logotypes (if previously loaded)

### What Requires Internet
- ❌ Initial data load
- ❌ Syncing changes to Airtable
- ❌ Loading new cover images

### Sync Queue

When offline, status changes are saved locally and queued. When you're back online:
1. The app automatically detects connection
2. Shows "同期中..." (Syncing) status
3. Uploads all queued changes to Airtable
4. Refreshes data to get any changes made elsewhere

## Customization

### Changing Colors

Edit the CSS variables in the `<style>` section:

```css
:root {
    --bg-primary: #0a0a0a;      /* Main background */
    --accent: #d4af37;          /* Gold accent color */
    --status-want: #c77dff;     /* Want status color */
    --status-pending: #f72585;  /* Pending status color */
    /* ... etc */
}
```

### Changing Default Filter

In the JavaScript section, find:

```javascript
let currentFilter = ['want', 'pending'];
```

Change to whatever status values you want shown by default.

### Adjusting Sort Order

In the `filterRecords()` function, modify the sort logic to change grouping/ordering.

## Troubleshooting

### App Won't Load Data
1. Check that you have internet connection for first load
2. Verify your API token is correct
3. Check browser console for errors (Safari → Develop → Show Web Inspector)

### Changes Not Syncing
1. Ensure you have internet connection
2. Check the sync indicator in top-right (should turn green when online)
3. Force refresh: pull down on the volume list

### Images Not Loading
1. First-time image load requires internet
2. Once loaded, images are cached for offline use
3. Large images may take time on slow connections

### App Not Installing on iPhone
1. Must be opened in Safari (not Chrome or other browsers)
2. URL must use HTTPS (not HTTP)
3. The manifest.json must be in the same directory

## Data Privacy

- All data is stored locally on your device (IndexedDB)
- No data is sent anywhere except to your Airtable base
- The app does not collect analytics or telemetry
- Your API token is stored only in the HTML file (consider security options above)

## Browser Compatibility

Tested and optimized for:
- ✅ iOS Safari 14+
- ✅ Chrome for Android
- ✅ Desktop browsers (for testing)

## Future Enhancements

Possible additions you could make:
- Quick-add new volumes form
- Barcode scanner integration (ISBN lookup)
- Notes field for each volume
- Export to CSV
- Statistics dashboard
- Price tracking
- Reading progress tracking

## Support

This is a custom-built tool for your specific Airtable schema. If you need to modify it for schema changes:

1. Update field names in the JavaScript section
2. Adjust the detail view in `openModal()` function
3. Modify sorting/grouping in `filterRecords()` function

## License

Custom-built for personal use. Modify as needed for your collection tracking needs!
