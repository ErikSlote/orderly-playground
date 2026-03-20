# Orderly — UI Prototypes

Two fully interactive prototypes for Orderly's voice-first restaurant ordering platform. Customer-facing ordering app + restaurant management dashboard. Zero frameworks, zero build steps, just open in a browser.

## Quick Start

```bash
# That's it. No install needed.
open index.html        # Customer ordering prototype
open dashboard.html    # Restaurant dashboard
```

Or deploy to any static host (GitHub Pages, Vercel, Netlify) and share URLs.

---

## Files

```
index.html           Customer-facing ordering app (1,769 lines)
dashboard.html       Restaurant management dashboard (2,114 lines)
README.md            This file
```

## Dependencies

### Customer Prototype (`index.html`)
| Dependency | Type | Required | Fallback |
|---|---|---|---|
| Unsplash | Image URLs | No | Colored gradient placeholders via `onerror` |

**That's it.** System fonts (`-apple-system`), vanilla JS, inline CSS. Works offline.

### Dashboard (`dashboard.html`)
| Dependency | Type | Required | Fallback |
|---|---|---|---|
| Google Fonts (DM Sans) | CDN stylesheet | No | System sans-serif |
| Unsplash | Image URLs | No | Colored gradient placeholders via `onerror` |
| Web Audio API | Browser built-in | No | Silent (try/catch wrapped) |

**No npm. No node_modules. No build step. No React/Vue/Angular.**

---

## Customer Prototype — Architecture

### Config
Everything restaurant-specific is in one object at the top of the `<script>`:

```javascript
const RESTAURANT = {
  name: 'A1 Snacks',
  table: 'Table 1',
  hours: 'Open until 10 PM',
  accent: '#e8650a',
  currency: '$',      // Change to PHP for Philippines, AED for Dubai
  taxRate: 0.08,
};
```

### Menu Data
`menuData` object below the config. Each item:
```javascript
{
  id: 'b1',
  name: 'Classic Burger',
  desc: 'Description',
  price: 12.99,
  css: 'food-burger',           // CSS class for gradient fallback color
  tags: ['P', 'GF'],            // V, VG, GF, DF, NF, S, P, N
  img: 'https://unsplash...',   // Optional, has onerror fallback
}
```

### Voice AI Integration Points
Search for `VOICE AI INTEGRATION` or `goVoice()` in the source:

```javascript
// Called when user taps "Start Voice Order"
function goVoice() {
  // >>> INIT YOUR SDK HERE (VAPI, Deepgram, etc.) <<<
  // The demo uses setTimeout to simulate - replace with real callbacks
}

// Call these from your SDK callbacks:
addItemToCart(item)       // When AI detects an order item
updateTranscript(text)    // Live speech-to-text output
toggleMute()              // Pause/resume
exitVoice()               // End session, return to default bar
```

### Features
- Homescreen: time-aware greeting, animated logo, rotating AI prompt suggestions
- Voice ordering: waveform bar, mute, live transcript, end session
- Text chat: keyword-matched AI responses with inline "Add to cart" buttons
- Menu: category pills, dietary filter (GF/V/VG/DF/NF/S), food photos, allergen tags
- Item detail: bottom sheet with 8 modifier toggles, special instructions, quantity
- Cart: draggable drawer, auto-scale by item count, tip selector (0/15/18/20%)
- Order confirmation: animated checkmark, itemized receipt, order number
- Order tracking: animated 4-step timeline, queue position display, "Order more" button
- Account: guest signup, editable profile (allergies, diet, notes, language)
- Themes: light/dark/system with full color coverage
- Responsive: strips phone frame on real devices (under 420px)

### Responsive Behavior
- Desktop: renders inside a 375px phone frame for demos
- Mobile (under 420px): phone frame removed, full-screen rendering, 100dvh height
- iOS safe areas supported via env(safe-area-inset-*)

---

## Dashboard — Architecture

### Pages (9)
| Page | Description |
|---|---|
| **Orders** | Kanban board (New, Accepted, Preparing, Ready, Served), drag-drop, detail modal, one-tap advance |
| **86 Board** | Items grouped by category, toggle on/off, time-since-86'd, sort/filter |
| **Menu** | Full CRUD: add/edit/delete items with modifiers, allergens, tags, image URL |
| **Tables** | Grid with status (empty/occupied/waiting), add/remove tables, detail panel |
| **Analytics** | Heatmap, hot sellers/underperformers, repeat customer favorites, customer insights, order method breakdown, date range filter |
| **History** | Searchable/filterable order history, expandable details, aggregate stats, CSV export |
| **QR Codes** | Per-table QR generation (SVG), copy URL, print |
| **Billing** | Plan details, usage stats (voice/text/browse breakdown), invoice history |
| **Settings** | Profile, hours, branding, tax rate, notifications, integrations, staff PINs, danger zone |

### Key Features
- **Live demo mode**: Press D or tap "Demo" to auto-simulate a dinner rush
- **Sound alerts**: Web Audio API chime on new orders (toggle on/off)
- **Notification banner**: Slides down when new orders arrive
- **Print receipt**: Opens formatted kitchen ticket in new window
- **Theme toggle**: Light/dark mode via top bar
- **Confirmation dialogs**: For destructive actions (decline, delete, close table)
- **Action toasts**: Every action shows feedback ("Order accepted", "Item 86'd", etc.)
- **Skeleton loading**: Page transitions shimmer briefly
- **Mobile responsive**: Sidebar collapses on tablets, orders switch to stacked card list on phones
- **iOS/iPad optimized**: Safe areas, touch targets, momentum scrolling

### Keyboard Shortcuts
| Key | Action |
|---|---|
| N | Simulate new order |
| D | Toggle demo mode |
| Escape | Close any modal + stop demo |

### Demo Data
4 pre-loaded orders in different stages, 15 menu items with photos, 12 tables, 12 past orders in history. All data is in-memory JS, no persistence, resets on refresh.

---

## For Production — What Needs to Change

These prototypes are **design references**, not production code. Here's what needs to be swapped out:

### Customer App
1. **Voice SDK**: Replace the setTimeout demo in goVoice() with VAPI/Deepgram/AssemblyAI
2. **Backend API**: Menu data should load from your API, not the hardcoded menuData object
3. **Order submission**: placeOrder() currently moves items to orderedItems array. Wire to order API
4. **Order tracking**: Timeline is demo-animated. Replace with websocket/polling from kitchen status
5. **Account system**: userProfile is in-memory JS. Needs auth backend (magic link/SMS)
6. **Real images**: Replace Unsplash URLs with restaurant's actual food photos

### Dashboard
1. **Auth**: No login. Needs auth middleware
2. **Real-time orders**: Replace demo data with websocket connection to order stream
3. **Database**: All data is in-memory arrays. Needs Postgres/Supabase/Firebase
4. **86 Board sync**: Toggle state needs to propagate to customer-facing menu in real-time
5. **Analytics**: Static demo data. Needs actual order aggregation queries
6. **QR codes**: SVG placeholders. Use a real QR library (qrcode.js) with actual URLs
7. **Payments/Billing**: Static mockup. Needs Stripe integration
8. **File upload**: "Import PDF" is a toast placeholder. Needs OCR pipeline for menu extraction

### What You Can Keep
- All CSS and layout: production-ready responsive design
- Component structure: every UI component maps to a real feature
- Data shapes: the JS objects mirror what your API responses should look like
- UX flows: every interaction has been thought through

---

## Deploying to GitHub Pages

```bash
git init
git add index.html dashboard.html README.md
git commit -m "Orderly prototypes"
git remote add origin https://github.com/YOUR_USERNAME/orderly-prototype.git
git push -u origin main
```

Then: Settings, Pages, Source: main branch, Save

- Customer app: https://YOUR_USERNAME.github.io/orderly-prototype/
- Dashboard: https://YOUR_USERNAME.github.io/orderly-prototype/dashboard.html

---

## Browser Support
- Chrome 90+, Safari 15+, Firefox 90+, Edge 90+
- iOS Safari (iPhone + iPad)
- Android Chrome
- Tablet-optimized (iPad landscape/portrait)

---

## Demo Script for Restaurant Pitches

1. Open dashboard.html on a laptop
2. Press D to start demo mode. Orders start flowing in
3. Walk through: accept an order, start prep, mark ready, served
4. Show 86 Board: toggle an item off
5. Show Analytics: heatmap, hot sellers, customer insights
6. Switch to index.html on a phone
7. Tap "Start Voice Order": demo AI adds items automatically
8. Browse menu: show dietary filters, item detail sheet
9. Place order: show confirmation screen and order tracking
10. Show Account page: allergies, dietary preferences

Total demo time: roughly 3 minutes. Covers the full product story.
