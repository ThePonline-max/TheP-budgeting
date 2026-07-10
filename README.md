# Budget Tracker — Native App Deployment Guide

## Overview
This project wraps your Budget Tracker PWA into native Android and iOS apps
using **Capacitor** by Ionic. The result is a real app you can submit to
the Google Play Store and Apple App Store.

---

## Prerequisites

Install these on your computer:

| Tool | Purpose | Install |
|------|---------|---------|
| **Node.js 18+** | JavaScript runtime | [nodejs.org](https://nodejs.org) |
| **Android Studio** | Build Android APK/AAB | [developer.android.com/studio](https://developer.android.com/studio) |
| **Xcode 15+** (Mac only) | Build iOS IPA | Mac App Store |
| **Google Play Console** | Publish to Play Store | [$25 one-time](https://play.google.com/console/signup) |
| **Apple Developer Account** | Publish to App Store | [$99/year](https://developer.apple.com/programs/) |

---

## Quick Start (Both Platforms)

```bash
# 1. Navigate to the project folder
cd budget-tracker-native

# 2. Install dependencies
npm install

# 3. Initialize Capacitor (already configured, just sync)
npx cap sync

# 4. Add platforms
npx cap add android
npx cap add ios    # Mac only

# 5. Open in IDE
npx cap open android   # Opens Android Studio
npx cap open ios       # Opens Xcode (Mac only)
```

---

## Android Deployment (Play Store)

### Build the APK/AAB

```bash
# Open in Android Studio
npx cap open android
```

In Android Studio:
1. Wait for Gradle sync to complete
2. Go to **Build → Generate Signed Bundle / APK**
3. Choose **Android App Bundle (AAB)** for Play Store
4. Create or select a **keystore** (save this securely — you need it for every update!)
5. Select **release** build variant
6. Click **Finish** — your `.aab` file will be in `android/app/build/outputs/bundle/release/`

### Upload to Play Store

1. Go to [play.google.com/console](https://play.google.com/console)
2. Create a new app → fill in details
3. Go to **Production → Create new release**
4. Upload your `.aab` file
5. Fill in:
   - App name: **Budget Tracker**
   - Short description: *Track expenses, plan budgets, and forecast spending*
   - Category: **Finance**
   - Content rating: Complete the questionnaire
6. Add screenshots (at least 2 phone screenshots)
7. Submit for review (usually 1-3 days)

### Generate Screenshots
Take screenshots from your phone/emulator at these sizes:
- Phone: 1080 x 1920 px (or similar 16:9)
- Tablet (optional): 1200 x 1920 px

---

## iOS Deployment (App Store)

### Requirements
- A Mac with **Xcode 15+**
- **Apple Developer Account** ($99/year)
- An iPhone/iPad for testing (or use Simulator)

### Build the IPA

```bash
# Open in Xcode
npx cap open ios
```

In Xcode:
1. Select the **App** target
2. Set your **Team** (Apple Developer account) in Signing & Capabilities
3. Set **Bundle Identifier**: `com.budgettracker.app`
4. Select a device or "Any iOS Device"
5. Go to **Product → Archive**
6. Once archived, click **Distribute App → App Store Connect**
7. Upload

### Upload to App Store Connect

1. Go to [appstoreconnect.apple.com](https://appstoreconnect.apple.com)
2. Click **My Apps → + New App**
3. Fill in:
   - Name: **Budget Tracker**
   - Primary Language: English
   - Bundle ID: `com.budgettracker.app`
   - SKU: `budget-tracker-v1`
4. Add app information:
   - Description, keywords, category (Finance)
   - Screenshots (6.7" and 5.5" required)
   - App icon (1024x1024 px)
5. Select your uploaded build
6. Submit for review (usually 1-2 days)

---

## App Icon

The icon SVG is in `resources/icon.svg`. You need to generate PNG versions:

### Android (required sizes):
- `mipmap-mdpi`: 48x48
- `mipmap-hdpi`: 72x72
- `mipmap-xhdpi`: 96x96
- `mipmap-xxhdpi`: 144x144
- `mipmap-xxxhdpi`: 192x192

### iOS (required sizes):
- 1024x1024 (App Store)
- 180x180 (iPhone @3x)
- 120x120 (iPhone @2x)
- 167x167 (iPad Pro)
- 152x152 (iPad)

**Easy way:** Use [appicon.co](https://www.appicon.co/) — upload the 1024x1024 PNG
and it generates all sizes for both platforms.

---

## App Signing

### Android Keystore
```bash
keytool -genkey -v -keystore budget-tracker.keystore \
  -alias budget-tracker -keyalg RSA -keysize 2048 -validity 10000
```
⚠️ **Keep your keystore file safe!** You cannot update your app without it.

### iOS
Handled automatically through Xcode when you set up your Apple Developer team.

---

## Alternative: PWABuilder (Easiest)

If you prefer not to set up Android Studio / Xcode:

1. First deploy to GitHub Pages (upload index.html to a repo, enable Pages)
2. Go to [pwabuilder.com](https://www.pwabuilder.com/)
3. Enter your GitHub Pages URL
4. Click **Package for stores**
5. Download:
   - Android → ready-to-upload AAB file
   - iOS → Xcode project (still needs a Mac to build)

---

## Project Structure

```
budget-tracker-native/
├── www/
│   └── index.html          ← Your app (the single-file PWA)
├── resources/
│   └── icon.svg            ← App icon source
├── package.json            ← Node.js dependencies
├── capacitor.config.json   ← Capacitor settings
└── README.md               ← This file
```

After running `npx cap add android/ios`, you'll also have:
```
├── android/                ← Android Studio project
└── ios/                    ← Xcode project
```

---

## Store Listing Copy

### Title
Budget Tracker — Expenses, Budgets & Savings

### Short Description (80 chars)
Track expenses, set budgets, forecast spending, and hit savings goals.

### Full Description
Budget Tracker is your personal finance companion. Log expenses with smart
auto-categorisation, set monthly budget limits per category, track recurring
bills, save towards goals, and forecast whether you'll stay within budget
by month end.

Features:
• Log expenses instantly with auto-category detection
• Track income from multiple sources
• Set monthly budgets per category with visual progress bars
• Recurring bills tracker with monthly total calculation
• Savings goals with progress tracking
• End-of-month spending forecast
• Built-in currency converter (GBP, USD, EUR, NGN + more)
• Works offline — your data stays on your device
• No account required — 100% private

Perfect for personal budgeting, tracking household expenses, and reaching
your financial goals.

### Keywords
budget, expense tracker, money, savings, finance, personal finance,
budget planner, spending tracker, currency converter

### Category
Finance

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `cap sync` fails | Run `npm install` first |
| Android build errors | Update Android Studio + Gradle |
| iOS signing issues | Check Apple Developer account + Xcode team settings |
| App crashes on launch | Check `logcat` (Android) or Xcode console (iOS) |
| Data not persisting | localStorage works in Capacitor WebView — should be fine |

---

## Support

Built with Capacitor 5.x. For updates:
- [capacitorjs.com/docs](https://capacitorjs.com/docs)
- [developer.android.com](https://developer.android.com)
- [developer.apple.com](https://developer.apple.com)
