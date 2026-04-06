# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a minimal Progressive Web App (PWA) template designed to be packaged as a Trusted Web Activity (TWA) for deployment to Android and Windows platforms. The project is a static HTML/JS application with no build process or dependencies.

**Deployment Target:** GitHub Pages at `https://mrmuralisg.github.io/HelloWorldTWA/`

## Development Commands

This is a static PWA with no build system. Development is done by:

1. **Local Testing:** Use any static file server, for example:
   ```bash
   python3 -m http.server 8000
   # or
   npx serve
   ```

2. **Testing PWA Features:** Must be served over HTTPS or localhost. Use browser DevTools > Application tab to inspect:
   - Manifest registration
   - Service Worker status
   - PWA installability

3. **Deployment:** Push to GitHub and serve via GitHub Pages (no build step required)

## Architecture

### File Structure
```
/
├── index.html           # Main entry point (minimal HTML shell)
├── service-worker.js    # PWA service worker (basic skeleton)
├── manifest.json        # PWA manifest (MALFORMED - see below)
├── manifestbkp1.json    # Clean working manifest
├── manifestbkp2.json    # Backup manifest (also malformed)
├── icon-192.png         # App icon (192x192)
├── icon-512.png         # App icon (512x512)
└── README.md            # Minimal project readme
```

### Critical Issue: Malformed Manifest

**manifest.json** has an invalid nested structure with a `"name"` object containing another `"name"` field (lines 2-49). This is not valid PWA manifest format and will cause installation failures.

**manifestbkp1.json** contains the correct, working manifest structure:
- Flat structure with proper top-level fields
- Name: "Hello World TWA"
- Short name: "HelloTWA"
- Theme: Green (#4CAF50)
- Relative icon paths

When working with the manifest, reference **manifestbkp1.json** as the correct template.

### Missing PWA Registration

**index.html** is missing critical PWA setup:
- No `<link rel="manifest" href="manifest.json">` tag
- No viewport meta tag
- No service worker registration script
- Service worker exists but is never registered

To make this a functional PWA, index.html needs:
```html
<link rel="manifest" href="manifest.json">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<script>
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/service-worker.js');
  }
</script>
```

### Service Worker Implementation

**service-worker.js** is a minimal skeleton that:
- Logs on install (line 1-3)
- Passes through all fetch requests without caching (line 5-7)

It does NOT provide offline functionality or asset caching. To add caching, implement proper cache strategies in the fetch handler.

## TWA Packaging Notes

When packaging as a TWA:
- The `start_url` in manifest points to the GitHub Pages URL
- Icons are referenced with absolute URLs in manifest.json but relative paths in manifestbkp1.json
- The `related_applications` field references Google Play Store (manifest.json:81-85)
- Ensure manifest is well-formed before building TWA wrapper

## Code Style

This project uses:
- Vanilla HTML/JS (no frameworks)
- Minimal code with template/placeholder structure
- No formatting configuration files present
