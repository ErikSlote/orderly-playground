# Orderly - Customer Ordering UI Prototype

A fully interactive front-end prototype for Orderly's voice-first restaurant ordering experience. Single HTML file, zero dependencies, works in any browser.

## What is this

An interactive mockup of the customer-facing ordering app that loads when someone scans a QR code at a restaurant table. Built as a design reference and demo tool for pitching to restaurants.

## Features

- **Home screen** with time-aware greeting, animated logo, rotating AI prompt suggestions
- **Voice ordering** with live waveform bar, mute toggle, real-time transcript, end session
- **Text chat** with AI responses, keyword detection, inline "Add to cart" buttons
- **Menu browsing** with category filtering, drag-scrollable category pills, food photos
- **Item detail sheet** with modifier toggles, special instructions input, quantity selector
- **Cart** with draggable bottom drawer (snap to half/full screen), quantity controls, remove items
- **Tip selector** (None / 15% / 18% / 20%) with live total updates
- **Order confirmation** screen with animated checkmark, itemized receipt, order number
- **Order tracking** with animated 4-step timeline (Ordered > Kitchen received > Preparing > Served)
- **Queue position** display (your order is Xth in line, X queued / X preparing / X served)
- **Order more** button to add additional rounds without re-scanning
- **Account system** with guest state, signup flow, and full settings editing:
  - Profile (name, contact)
  - Allergies (8 options, toggle on/off)
  - Dietary preferences (8 options)
  - Custom notes (free text)
  - Language selection (8 languages)
  - Order history link
  - Sign out / Delete data
- **Light / Dark / System theme** switching with full color coverage
- **Allergen/dietary tags** on menu items (Popular, Vegetarian, Gluten-free, Spicy, etc.)
- **Responsive** - strips phone frame on real mobile devices (under 420px width)

## Dependencies

**None.** This is a single self-contained HTML file with inline CSS and JavaScript. No build step, no npm, no frameworks.

The only external resources are Unsplash image URLs for food photos. These are optional - every image has an `onerror` fallback that shows a colored gradient placeholder if images fail to load. The prototype works fully offline except for the photos.

## How to run

Open `index.html` in any browser. That's it.

To test on a real phone: host the file anywhere (GitHub Pages, Vercel, Netlify, or even `python3 -m http.server`) and open the URL on your phone. The responsive layout strips the phone frame and renders full-screen.

## How to customize for a different restaurant

Edit the `RESTAURANT` config object at the top of the `<script>` section:

```javascript
const RESTAURANT = {
  name: 'A1 Snacks',       // Restaurant name (shows in header + home)
  table: 'Table 1',         // Table identifier
  hours: 'Open until 10 PM', // Operating hours display
  accent: '#e8650a',        // Brand color (orange by default)
  currency: '$',             // Currency symbol (USD, PHP, AED, etc.)
  taxRate: 0.08,            // Tax rate (8% default)
};
```

To change the menu, edit the `menuData` object below the config. Each item has:

```javascript
{
  id: 'b1',                    // Unique ID
  name: 'Classic Burger',      // Display name
  desc: 'Description text',    // Short description
  price: 12.99,               // Price (in restaurant's currency)
  css: 'food-burger',         // CSS class for gradient fallback color
  tags: ['P', 'GF'],          // Dietary/allergen tags
  img: 'https://...'          // Image URL (optional, has fallback)
}
```

Available tags: `V` (Vegetarian), `VG` (Vegan), `GF` (Gluten-free), `DF` (Dairy-free), `NF` (Nut-free), `S` (Spicy), `P` (Popular), `N` (New)

## Voice AI integration

The prototype includes placeholder code for voice AI integration. Search for `VOICE AI INTEGRATION` in the source to find the hook points:

- `goVoice()` - Initialize your voice SDK (VAPI, Deepgram, etc.)
- `addItemToCart(item)` - Call when AI detects an order item
- `updateTranscript(text)` - Call with live speech-to-text output
- `toggleMute()` - Pause/resume voice stream
- `exitVoice()` - Disconnect SDK and return to default bar

The demo simulates AI behavior with setTimeout - replace those with real SDK callbacks.

## File structure

```
orderly-prototype/
  index.html      # The entire prototype (single file)
  README.md       # This file
```

## Tech stack

- HTML5
- CSS3 (no preprocessor)
- Vanilla JavaScript (no libraries)
- Unsplash (image URLs only, with fallbacks)

## Browser support

- Safari iOS 15+
- Chrome Android 8+
- All modern desktop browsers
- Responsive layout for screens under 420px

## GitHub Pages deployment

1. Push to a GitHub repo
2. Go to Settings > Pages
3. Set source to main branch, root folder
4. Your prototype will be live at `https://yourusername.github.io/repo-name/`

## License

Built as a UI/UX reference for Orderly AI. Internal use.
