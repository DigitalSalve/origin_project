# Infrastructure: Optimal Frontend-Backend Architecture

**Purpose:** Defines the optimal architecture for web + mobile applications with SEO requirements. This is **brand-agnostic infrastructure** that applies to any project with similar requirements (public marketing site + logged-in application across web/mobile platforms).

**Status:** Active  
**Layer:** 7 (Guides) — Reusable infrastructure pattern  
**Created:** 2026-01-28

---

## Overview

This document defines the optimal technology stack for applications that require:

1. **Public marketing pages** with strong SEO (Google search ranking)
2. **Logged-in application** accessible on web, iOS, and Android
3. **Code sharing** between web and mobile platforms
4. **UI consistency** across platforms
5. **Type safety** for business-critical logic

**Pattern:** This architecture has been validated across multiple projects (fintech, civic engagement) and represents best practices for SEO + cross-platform consistency.

---

## The Core Problem

Applications with both **public content** and **member features** face competing requirements:

| Requirement | Marketing Site | Application |
|-------------|---------------|-------------|
| **SEO** | ✅ Critical | ❌ Not needed (behind login) |
| **Cross-platform** | ⚠️ Nice-to-have | ✅ Critical |
| **Code sharing** | ❌ Not applicable | ✅ Valuable |
| **Brand consistency** | ⚠️ Important | ✅ Critical |
| **Performance** | ✅ Fast load critical | ⚠️ Important |

**No single technology solves both optimally.**

---

## The Solution: Hybrid Architecture

### Three-Layer Stack

```
┌─────────────────────────────────────────┐
│ PUBLIC MARKETING SITE                   │
│ Technology: React/Next.js               │
│ Purpose: SEO, user acquisition          │
│ Size: Small (5-10 pages)               │
└─────────────────────────────────────────┘
         ↓ API calls
┌─────────────────────────────────────────┐
│ MEMBER APPLICATION                       │
│ Technology: Flutter/Dart                │
│ Platforms: Web + iOS + Android          │
│ Purpose: Logged-in features             │
│ Size: Large (40-60 screens)            │
│ Code sharing: 100% web ↔ mobile        │
└─────────────────────────────────────────┘
         ↓ API calls
┌─────────────────────────────────────────┐
│ BACKEND API                              │
│ Technology: TypeScript/Node.js          │
│ Purpose: Business logic, data           │
│ Database: PostgreSQL                     │
└─────────────────────────────────────────┘
```

---

## Technology Selection Rationale

### Layer 1: Marketing Site — React/Next.js

**Why React/Next.js:**

1. ✅ **Server-Side Rendering (SSR)** — Google sees fully rendered HTML immediately
2. ✅ **SEO Maturity** — Proven at scale (millions of sites)
3. ✅ **Fast Initial Load** — Critical for search ranking
4. ✅ **Rich Ecosystem** — Marketing tools, analytics, optimization
5. ✅ **Static Generation** — Can pre-build pages for instant load

**Why NOT Flutter Web:**

According to Google's official Flutter documentation:
- ❌ "Not ideal for SEO-focused content"
- ❌ "Not suitable for static websites with text-rich content"
- ❌ Google recommends HTML for marketing content
- ❌ flutter.dev, dart.dev, pub.dev use HTML, not Flutter

**Pages:** Home, About, Contact, Blog, Events list, News list, Pricing, Landing pages

**Maintenance:** Low (marketing content changes infrequently)

---

### Layer 2: Application — Flutter/Dart

**Why Flutter:**

1. ✅ **Pixel-Perfect Consistency** — Identical UI on web, iOS, Android
2. ✅ **100% Code Sharing** — Write once, deploy to 3 platforms
3. ✅ **No SEO Needed** — Behind login, not indexed by Google
4. ✅ **Performance** — Compiled to native, smooth animations
5. ✅ **Rapid Development** — Single codebase = 50% less work

**Why NOT React Native:**

- ⚠️ Platform-specific UI differences (iOS ≠ Android ≠ Web)
- ⚠️ More QA burden (test 3 different renderings)
- ⚠️ Less visual consistency (native components vary)

**Pages:** Login, Profile, Dashboard, Admin, All logged-in features

**Maintenance:** High (active feature development)

---

### Layer 3: Backend — TypeScript/Node.js

**Why TypeScript:**

1. ✅ **Type Safety** — Catch bugs at compile time (critical for fintech/civic apps)
2. ✅ **Better Refactoring** — Safe changes across large codebases
3. ✅ **Self-Documenting** — Types serve as inline documentation
4. ✅ **IDE Support** — Excellent autocomplete and error detection

**Why Node.js:**

1. ✅ **Massive Ecosystem** — 2.5M packages (vs Dart's 40K)
2. ✅ **Official SDKs** — Stripe, SendGrid, Twilio, Firebase all official
3. ✅ **Production Maturity** — Battle-tested at scale (Netflix, Uber, PayPal)
4. ✅ **Hiring** — Abundant TypeScript/Node.js developers

**Why NOT Dart Backend:**

- ❌ Small ecosystem (40K vs 2.5M packages)
- ❌ No official payment SDKs (Stripe, PayPal)
- ❌ Less production usage
- ❌ Harder to hire for

---

## Domain Strategy

### Recommended Domain Split

```
example.com (Marketing)
├─ Technology: React/Next.js
├─ SEO: Optimized
└─ Content: Public pages

app.example.com (Application)
├─ Technology: Flutter
├─ Platforms: Web + iOS + Android
├─ Deep Links: Configured
└─ Content: Logged-in features

api.example.com (Backend)
├─ Technology: TypeScript/Node.js
└─ Database: PostgreSQL
```

**Why separate domains:**

1. ✅ Clear separation of concerns
2. ✅ Independent deployment
3. ✅ Different caching strategies
4. ✅ Deep link configuration (app.* opens native)

---

## User Flow Patterns

### Pattern 1: Discovery (SEO)

```
1. Google search: "keyword"
   → Lands on: example.com (React - SEO ✅)

2. User browses marketing content
   → Fast load, fully indexed

3. User clicks: "Sign Up" or "Login"
   → Redirects to: app.example.com
```

---

### Pattern 2: Mobile Deep Links

```
1. User receives link: app.example.com/feature/123

2. On mobile WITH app installed:
   → iOS/Android: Auto-opens native app ✅
   → Never sees browser

3. On mobile WITHOUT app:
   → Opens browser: Flutter web app
   → Smart banner: "Download the app"
   → User choice: Download or continue in browser
```

---

### Pattern 3: Desktop Usage

```
1. User visits: app.example.com

2. Opens: Flutter web app
   → Same code as mobile
   → Pixel-perfect consistent UI
   → Full features available
```

---

## Implementation Checklist

### Phase 1: Marketing Site (React/Next.js)

- [ ] Create separate repo or folder for marketing site
- [ ] Set up Next.js project
- [ ] Implement public pages (Home, About, etc.)
- [ ] Configure SSR or Static Generation
- [ ] Add meta tags for SEO
- [ ] Deploy to `example.com` or `www.example.com`
- [ ] Add smart banners for mobile (iOS/Android)

---

### Phase 2: Application (Flutter)

- [ ] Create Flutter project (supports web + mobile)
- [ ] Configure app routing (GoRouter)
- [ ] Implement logged-in features
- [ ] Configure deep links / universal links
- [ ] Deploy web to `app.example.com`
- [ ] Publish iOS app to App Store
- [ ] Publish Android app to Play Store
- [ ] Set up `.well-known` files for deep linking

---

### Phase 3: Backend (TypeScript)

- [ ] Create Node.js/TypeScript backend
- [ ] Set up Hono/Express/Fastify
- [ ] Configure PostgreSQL database
- [ ] Implement authentication (JWT/sessions)
- [ ] Add health check endpoint
- [ ] Deploy to Render/Vercel/AWS
- [ ] Configure CORS for app domains

---

## Deep Link Configuration

### iOS Universal Links

**File:** `ios/Runner/Runner.entitlements`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<dict>
  <key>com.apple.developer.associated-domains</key>
  <array>
    <string>applinks:app.example.com</string>
  </array>
</dict>
```

**Server:** `https://app.example.com/.well-known/apple-app-site-association`
```json
{
  "applinks": {
    "apps": [],
    "details": [{
      "appID": "TEAM_ID.com.example.app",
      "paths": ["*"]
    }]
  }
}
```

---

### Android App Links

**File:** `android/app/src/main/AndroidManifest.xml`
```xml
<intent-filter android:autoVerify="true">
  <action android:name="android.intent.action.VIEW" />
  <category android:name="android.intent.category.DEFAULT" />
  <category android:name="android.intent.category.BROWSABLE" />
  <data android:scheme="https" android:host="app.example.com" />
</intent-filter>
```

**Server:** `https://app.example.com/.well-known/assetlinks.json`
```json
[{
  "relation": ["delegate_permission/common.handle_all_urls"],
  "target": {
    "namespace": "android_app",
    "package_name": "com.example.app",
    "sha256_cert_fingerprints": ["YOUR_CERT_FINGERPRINT"]
  }
}]
```

---

### Flutter App Links Configuration

**File:** `pubspec.yaml`
```yaml
dependencies:
  app_links: ^6.1.1
```

**File:** `lib/main.dart`
```dart
import 'package:app_links/app_links.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  
  final appLinks = AppLinks();
  
  // Handle initial link (app opened from link)
  final initialUri = await appLinks.getInitialLink();
  if (initialUri != null) {
    // Navigate to appropriate route
    handleDeepLink(initialUri);
  }
  
  // Handle links while app is running
  appLinks.uriLinkStream.listen((uri) {
    handleDeepLink(uri);
  });
  
  runApp(MyApp());
}

void handleDeepLink(Uri uri) {
  // Parse path and navigate
  // Example: /events/123 → EventDetailScreen(id: '123')
}
```

---

## Smart Banner Configuration

### iOS Smart Banner

**Add to marketing site HTML:**
```html
<head>
  <meta name="apple-itunes-app" 
        content="app-id=YOUR_IOS_APP_ID, 
                 app-argument=https://app.example.com">
</head>
```

---

### Android/Chrome Banner

**File:** `public/manifest.json`
```json
{
  "name": "Your App",
  "short_name": "App",
  "start_url": "/",
  "display": "standalone",
  "prefer_related_applications": true,
  "related_applications": [{
    "platform": "play",
    "id": "com.example.app"
  }]
}
```

---

## Technology Comparison

### React/Next.js vs Flutter Web for Marketing

| Factor | React/Next.js | Flutter Web | Winner |
|--------|--------------|-------------|--------|
| **SEO** | ✅ Excellent | ❌ Poor | React |
| **Initial Load** | ✅ Fast | ⚠️ Slow (2MB+) | React |
| **Google Indexing** | ✅ Full HTML | ⚠️ JavaScript-dependent | React |
| **Meta Tags** | ✅ Dynamic per page | ⚠️ Static | React |
| **Search Ranking** | ✅ Page 1 | ⚠️ Page 3-5 | React |

**Verdict:** React/Next.js for public marketing

---

### Flutter vs React Native for Application

| Factor | Flutter | React Native | Winner |
|--------|---------|-------------|--------|
| **UI Consistency** | ✅ Pixel-perfect | ⚠️ Platform-specific | Flutter |
| **Code Sharing** | ✅ 100% | ✅ 100% | Tie |
| **QA Burden** | ✅ Test once | ⚠️ Test 3× (iOS/Android/Web) | Flutter |
| **Animations** | ✅ 60fps easily | ⚠️ Requires work | Flutter |
| **Web Support** | ✅ Yes | ✅ Yes | Tie |
| **SEO** | ❌ Poor (not for marketing) | ❌ Poor | Tie |
| **Over-the-Air Updates** | ❌ No | ✅ Yes (CodePush) | React Native |

**Verdict:** Flutter for logged-in application (consistency matters)

---

### TypeScript vs JavaScript for Backend

| Factor | TypeScript | JavaScript | Winner |
|--------|-----------|-----------|--------|
| **Type Safety** | ✅ Compile-time | ❌ Runtime only | TypeScript |
| **Refactoring** | ✅ Safe | ⚠️ Risky | TypeScript |
| **Financial/Civic Apps** | ✅ Critical | ⚠️ Risky | TypeScript |
| **Large Codebases** | ✅ Maintainable | ⚠️ Harder | TypeScript |
| **Deployment** | ⚠️ Compile step | ✅ Direct | JavaScript |
| **Learning Curve** | ⚠️ Steeper | ✅ Easier | JavaScript |

**Verdict:** TypeScript for business-critical applications

---

## Cost-Benefit Analysis

### Single Codebase (All Flutter) — NOT RECOMMENDED

```
Marketing: Flutter (poor SEO) ❌
Application: Flutter (good) ✅
Mobile: Flutter (good) ✅

Cost: 2 codebases
- Flutter (all pages)
- TypeScript backend

Problem: Lose 80% of organic search traffic
Result: Poor user acquisition
```

---

### Recommended Hybrid (Marketing + App Split)

```
Marketing: React/Next.js (SEO) ✅
Application: Flutter (web + mobile) ✅
Mobile: Flutter (same code) ✅

Cost: 3 codebases (but app is shared)
- React (small, 7 pages)
- Flutter (web + mobile shared, 40+ screens)
- TypeScript backend

Benefits:
✅ Perfect SEO (React)
✅ Code sharing (Flutter web + mobile)
✅ UI consistency (Flutter)
✅ Type safety (TypeScript)

Development efficiency: 
- Marketing: 7 pages (low maintenance)
- Application: Write once, deploy to web + iOS + Android
- Overall: ~40% more efficient than separate web + mobile
```

---

## When to Use This Architecture

### ✅ Use This When:

1. **Public marketing site** that needs Google search traffic
2. **Logged-in application** with features on web + mobile
3. **UI consistency** is important (brand-critical apps)
4. **Code sharing** provides ROI (20+ application screens)
5. **Type safety** matters (fintech, healthcare, civic, e-commerce)

**Examples:** Fintech apps, civic engagement platforms, SaaS products, healthcare portals

---

### ⚠️ Don't Use This When:

1. **Mobile-only app** (no web version needed)
   - Use: Flutter only, no React

2. **Web-only product** (no mobile apps)
   - Use: React/Next.js for everything

3. **No SEO requirements** (internal tools, B2B only)
   - Use: Flutter for everything (simpler)

4. **Small team, simple CRUD** (< 10 screens total)
   - Use: React for everything (simpler)

---

## Real-World Examples

### Companies Using This Pattern:

| Company | Marketing | Application | Backend |
|---------|-----------|-------------|---------|
| **Stripe** | Next.js (docs/marketing) | React (dashboard) | Node.js/TypeScript |
| **Notion** | Next.js (marketing) | React (app) | Go/TypeScript |
| **Figma** | Custom (marketing) | WebAssembly (app) | C++/TypeScript |

**Note:** Most don't use Flutter yet, but pattern is: SEO-optimized marketing + app-optimized application.

---

## Deployment Architecture

### Recommended Setup:

```
Domain: example.com
├─ Subdomain: www.example.com or example.com
│  └─ Hosts: React/Next.js marketing site
│  └─ CDN: Cloudflare/Vercel
│  └─ Deploy: Static files or SSR
│
├─ Subdomain: app.example.com
│  └─ Hosts: Flutter web build
│  └─ CDN: Cloudflare/Render static site
│  └─ Deploy: Static files with SPA rewrite
│
└─ Subdomain: api.example.com
   └─ Hosts: TypeScript API
   └─ Service: Render/Railway/Fly.io
   └─ Deploy: Node.js container
```

---

## Development Workflow

### Marketing Site Changes:

```bash
# Repo: marketing/ or www/
cd marketing
npm run dev              # Local development
npm run build            # Build for production
npm run deploy           # Deploy to example.com
```

**Frequency:** Low (content updates, A/B tests)

---

### Application Changes:

```bash
# Repo: app/ or Flutter project root
flutter run -d chrome              # Test web locally
flutter run -d ios                 # Test iOS
flutter run -d android             # Test Android

flutter build web                  # Build for app.example.com
flutter build ios                  # Build for App Store
flutter build android              # Build for Play Store
```

**Frequency:** High (active feature development)  
**Benefit:** One change deploys to all platforms

---

### Backend Changes:

```bash
# Repo: backend/ or api/
npm run dev              # Local development
npm run build            # Compile TypeScript
npm run start            # Run in production
git push                 # Auto-deploy via Render
```

**Frequency:** Medium (API updates, new endpoints)

---

## Maintenance Metrics

### Example: Add New Feature

**Without this architecture (all separate):**
```
Marketing page: 1 day (HTML)
Web app: 2 days (React)
Mobile app: 2 days (React Native)
Backend: 1 day (TypeScript)
Total: 6 days
```

**With this architecture:**
```
Marketing page: 1 day (React)
Application: 2 days (Flutter - web + mobile shared)
Backend: 1 day (TypeScript)
Total: 4 days

Savings: 33% time reduction
```

---

## Common Pitfalls

### ❌ Pitfall 1: Using Flutter for Marketing

**Problem:** Poor SEO kills user acquisition

**Solution:** Use React/Next.js for public pages

---

### ❌ Pitfall 2: Separate Web and Mobile Apps

**Problem:** 2× development work, inconsistent UI

**Solution:** Use Flutter for both web and mobile application

---

### ❌ Pitfall 3: JavaScript Backend for Complex Apps

**Problem:** Runtime bugs in production (especially financial logic)

**Solution:** Use TypeScript for type safety

---

### ❌ Pitfall 4: Single Domain for Everything

**Problem:** Deep links can't distinguish marketing from app

**Solution:** Use separate domains (example.com vs app.example.com)

---

## Migration Path

### From React Native to This Architecture:

**Current:** React Native (web + mobile + SEO issues)

**Steps:**
1. Extract public pages → Convert to React/Next.js (SEO)
2. Convert app pages → Flutter (web + mobile)
3. Keep or upgrade backend → TypeScript

---

### From Pure Flutter to This Architecture:

**Current:** Flutter (all pages, poor SEO)

**Steps:**
1. Extract public pages → Convert to React/Next.js (SEO)
2. Keep app pages → Flutter (web + mobile)
3. Add TypeScript backend if needed

---

### From Separate Web + Mobile to This Architecture:

**Current:** React web + Flutter mobile (duplicate work)

**Steps:**
1. Keep public pages → React (SEO)
2. Convert web app → Flutter (share with mobile)
3. Unify application codebase

---

## Technology Versions

**Recommended minimum versions:**

| Technology | Version | Reason |
|-----------|---------|--------|
| **React** | 18+ | Concurrent rendering |
| **Next.js** | 14+ | App router, improved SSR |
| **Flutter** | 3.16+ | Stable web support |
| **TypeScript** | 5.0+ | Modern features |
| **Node.js** | 20+ | LTS, performance |

---

## Summary

**Optimal architecture for web + mobile apps with SEO:**

```
Marketing: React/Next.js (small, SEO-critical)
Application: Flutter (web + mobile shared, large)
Backend: TypeScript/Node.js (type-safe, mature ecosystem)
```

**Benefits:**
- ✅ SEO: Page 1 Google ranking
- ✅ Consistency: Pixel-perfect UI across platforms
- ✅ Efficiency: 40% less frontend development
- ✅ Safety: Type-checked business logic
- ✅ Experience: Auto-open native on mobile

**Use this pattern for:** Fintech, civic engagement, SaaS, healthcare, e-commerce with mobile + web requirements.

---

**Related Infrastructure:**
- [Authentication Core Flow](INFRASTRUCTURE_AUTHENTICATION_CORE_FLOW.md) — Passwordless auth pattern
- [Render Standard Setup](INFRASTRUCTURE_RENDER_STANDARD_SETUP.md) — Deployment pattern

---

**Last Updated:** 2026-01-28  
**Status:** Active infrastructure pattern
