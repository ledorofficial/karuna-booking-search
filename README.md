# Karuna Booking Search

A WordPress plugin for **karunasiargao.com** — a branded availability search that
replaces the Smoobu embed. A compact one-month calendar (live nightly prices,
sold-out nights greyed out) that sends the visitor to:

```
https://bookings.karunasiargao.com/?arrival=YYYY-MM-DD&departure=YYYY-MM-DD&guests=N#rooms
```

## Install

1. Download the latest zip from **[Releases](../../releases)** (or *Code → Download
   ZIP* and rename the folder to `karuna-booking-search`).
2. WP Admin → **Plugins → Add New → Upload Plugin** → the zip → **Install** →
   **Activate**.
3. Place it:
   - **Shortcode:** `[karuna_booking_search]` — in Elementor use a *Shortcode*
     element (never a Text / Heading / HTML widget)
   - **Widget:** *Appearance → Widgets → Karuna Booking Search*

## Updates

The plugin checks this repo's **[Releases](../../releases)**. When a release has a
higher version than what's installed, WordPress shows the normal **update
available** notice in *Plugins* and updates with one click — no re-upload.
(WordPress checks automatically every ~12h; force it from *Dashboard → Updates →
Check again*.)

Shipping an update = bump the `Version:` header in `karuna-booking-search.php`,
push, and publish a GitHub release tagged `vX.Y.Z`.

Uses [Plugin Update Checker](https://github.com/YahnisElsts/plugin-update-checker)
(bundled in `plugin-update-checker/`). Works token-free while this repo is public.

## Shortcode attributes

| Attribute    | Default                              | Notes                                 |
|--------------|--------------------------------------|---------------------------------------|
| `base`       | `https://bookings.karunasiargao.com/`| Where the search submits.              |
| `target`     | `_self`                              | `_blank` opens results in a new tab.   |
| `max_guests` | `16`                                 | Highest number in the People dropdown. |
| `prices`     | `on`                                 | `off` hides the per-night prices.      |

```
[karuna_booking_search max_guests="4"]     Skygazer (sleeps 4)
[karuna_booking_search prices="off"]        calendar without prices
```

## Backend dependency

The price calendar reads `GET /api/calendar` on the booking engine — the
lowest-nightly-rate map, as JSON, with an `Access-Control-Allow-Origin` header
for `karunasiargao.com`. If that endpoint or flatpickr's CDN is unreachable the
widget falls back to plain native date fields, so it always works.

That endpoint lives in the booking-engine repo (`bookings.karunasiargao.com`),
route `GET /api/calendar`, origins controlled by `CORS_ORIGINS`.

## No-plugin alternative

`elementor-html-widget.html` is the whole widget as one paste-in block (markup +
`<style>` + `<script>`, flatpickr from cdnjs) for an Elementor **HTML** widget or
a **Custom HTML** block — but it does not self-update.

## Files

| Path | Purpose |
|---|---|
| `karuna-booking-search.php` | Plugin bootstrap, shortcode, widget, update checker. |
| `assets/kbs.css`, `assets/kbs.js` | Widget styles + logic (`__KBS_API__` swapped for the endpoint URL at runtime). |
| `elementor-html-widget.html` | Standalone paste-in copy — keep in sync with `assets/*`. |
| `plugin-update-checker/` | Bundled updater library (MIT). |

## Styling

Brand tokens: brown `#9D6B2B`, dark teal `#00333D`, sand `#F0EADC`, font
**Trirong**. Override `.kbs-widget` / `.flatpickr-calendar.kbs-fp` in the theme's
Additional CSS.
