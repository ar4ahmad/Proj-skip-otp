# Tabby In-App Checkout — Station Screen Prototype

## Project Overview

Tabby is migrating from web-based checkout (webview inside merchant apps) to **in-app checkout**. When a customer selects Tabby at a merchant's checkout page, they see a "station screen" inside the merchant app. This screen guides them to:

1. Receive a push notification from the Tabby app
2. Tap notification → open Tabby app → select BNPL plan → complete payment
3. Return to the station screen where payment success is confirmed

This project is an **interactive prototype** of the station screen for developer handoff.

## Tech Stack

- **Single `index.html` file** — no build step, no framework, no dependencies
- Plain HTML + CSS + vanilla JS
- Inter font via Google Fonts CDN
- Run with any static server: `python3 -m http.server 8090`

## Architecture

State is managed via a `data-state="1|2|3"` attribute on `.iphone-frame`. All visual changes between states are driven purely by CSS descendant selectors (`[data-state="2"] .notification-banner { ... }`). JS is minimal — toggles the data attribute on button click + a resize listener that scales the phone to fit the viewport via a `--phone-scale` CSS custom property on `.phone-wrapper`.

## 3 Screen States

| State | Trigger | What changes |
|-------|---------|-------------|
| **1 — Initial** | Page load / Restart button | Spinner active, step 1 gray ("Sending push notification..."), steps 2-3 gray dots, no footer, no notification |
| **2 — Notification Sent** | "Send Notification" button | iOS notification slides in from top, step 1 turns green + text changes to "Tap the notification...", "Having trouble?" link appears, footer reveals with phone number |
| **3 — Purchase Complete** | "Finish Purchase" button | Notification slides away, all step dots cascade to green (staggered 0.2s delays), all lines fill green |

## Figma Source

File: `ekQkGLPpsC1RqPEO9mScsv` (In-app Checkout Flow — Skip OTP)

| Screen | Node ID | URL |
|--------|---------|-----|
| Screen 1 (Initial) | `1502:33448` | https://www.figma.com/design/ekQkGLPpsC1RqPEO9mScsv/?node-id=1502-33448 |
| Screen 2 (Notification) | `1502:33457` | https://www.figma.com/design/ekQkGLPpsC1RqPEO9mScsv/?node-id=1502-33457 |
| Screen 3 (Complete) | `1502:33479` | https://www.figma.com/design/ekQkGLPpsC1RqPEO9mScsv/?node-id=1502-33479 |

## Design Tokens

### Colors
| Token | Hex | Usage |
|-------|-----|-------|
| front-primary | `#1d2329` | Main text, headings |
| front-secondary | `#7f8b99` | Subtitles, descriptions |
| front-tertiary | `#b8c3d1` | Step 1 icon bg (initial state) |
| line-disabled | `#e9eff5` | Inactive dots, lines |
| line-positive | `#31cc7e` | Active/completed green |
| icon-circle-bg | `#f0e8f7` | Lavender circle behind arrow icon |
| arrow-icon | `#3d1560` | Dark purple arrow stroke |
| notification-bg | `rgba(80,79,79,0.7)` | iOS notification backdrop |

### Typography (all Inter)
| Style | Weight | Size | Line Height | Letter Spacing |
|-------|--------|------|-------------|----------------|
| H1 | 600 | 35px | 36px | -0.7px |
| Body 1 Bold | 700 | 16px | 20px | -0.16px |
| Body 1 | 500 | 16px | 20px | -0.16px |
| Body 2 | 500 | 14px | 20px | -0.16px |
| Caption | 500 | 12px | 16px | -0.13px |

## What's Built

- [x] iPhone frame with notch, status bar, Safari address bar, home indicator
- [x] NavBar (X close, "Adidas" + "Pay with tabby", "العربية")
- [x] Spinning loader (arrow icon + animated SVG progress ring)
- [x] Heading
- [x] 3-step tracker/timeline with icons, dots, connecting lines
- [x] iOS notification banner with backdrop blur + slide animation
- [x] Footer (phone number + consent text)
- [x] 3 control buttons to the left of phone frame, top-aligned
- [x] All CSS transitions between states (notification slide, green cascade, footer reveal)
- [x] Responsive layout — phone scales proportionally to fit viewport (height & width) via CSS custom property + JS resize listener

## What's NOT Built Yet

- [ ] **Account switcher** — bottom component where user can change their mobile number (pre-fed by merchant) if their Tabby account is on a different number
- [ ] **"Having trouble with notification?" bottom sheet** — triggered by the link in step 1 (state 2/3), allows user to resend notification or receive SMS instead

## Running

```bash
cd "/Users/aar/Library/CloudStorage/Dropbox/Personal/Claude stuff/Proj-skip-otp"
python3 -m http.server 8090
# Open http://localhost:8090
```

Or configure `.claude/launch.json` (already set up) and use the preview server.
