# Zeekret Maps Link (phone)

A small Android phone app for the **Zeekr 7X**. While **Google Maps** navigates on your phone (also through
**Android Auto**), it sends the **distance left on your route** and the **arrival time** to **Zeekret Dash** on the
car's head unit. Zeekret Dash then shows the **battery % you'll arrive with**, right on the car's screen:

**🔋 69% 🏁**

Change the destination in Google Maps and the car follows within seconds. No sharing, no typing on every trip.

> This repository holds **documentation only, no source code**. Zeekret Maps Link is closed-source.

> 🚗 **Tested with the Zeekr 7X only (SEA region).** Any other car or region is entirely at your own risk.

> 🧪 **Free, unofficial, personal project.** Not affiliated with Zeekr, Geely, ECARX or Google. No warranty and no
> responsibility for anything that happens from using it. The % is an estimate: always keep an eye on the car's own
> range.

## The Zeekret apps

| App | Runs on | What it does |
|---|---|---|
| **[Zeekret](https://github.com/pt-atlas/zeekret-updates)** | Phone + Wear OS watch | Bluetooth digital key, remote control, stats, widget |
| **[Zeekret Dash](https://github.com/pt-atlas/zeekret-dash)** | Car head unit | Blindspot camera, mirror, dash cam, car controls, **battery at arrival** |
| **Zeekret Maps Link** (this one) | Phone | Sends your live Google Maps route to Zeekret Dash |

You need **Zeekret Dash** in the car. Zeekret Maps Link on its own shows nothing.

## What it sends, and what it doesn't

- **Sent:** metres left on the route, the arrival time, and when it was sent. That's all.
- **Never sent:** your location, the destination, street names, turn instructions, contacts or anything else.
- The numbers are **locked (AES-256-GCM)** with a key only your phone and your car know, from the car's pair code.
  The server in between only passes the locked box along and **can't read it**. Old boxes are deleted after 24 h.
- When navigation ends, it tells the car to clear the route.

## How the % is worked out

Zeekret Dash takes the car's **battery %** and **range**, and the km left from Google Maps:

- With the car's **average consumption** (from the Zeekret phone app): battery % − km × kWh per 100 km ÷ battery size.
- Without it: battery % × (range − km left) ÷ range.
- **Orange** below 20 %, **red** below 5 %. If you won't make it, it shows **0 %** in red.
- With stops added in Google Maps, the % is for your **final destination**.

## Install

> ⚠️ Zeekret Maps Link reads Google Maps' navigation notification, so it needs **notification access**. Android
> treats that as a *restricted setting* for apps installed outside the Play Store, and **Google Play Protect may
> block** installing it. That's why setup takes a few more steps than a normal app.

1. **Install the APK** on your phone. If tapping it fails ("App not installed") or Play Protect blocks it, install it
   from a computer over USB: `adb install ZeekretMapsLink.apk`.
2. **Allow the restricted setting** (only if notification access is greyed out in step 4):
   - Most phones: **Settings ▸ Apps ▸ Zeekret Maps Link ▸ ⋮ (top right) ▸ Allow restricted settings**.
   - Phones that hide that menu (e.g. Xiaomi / HyperOS): from a computer with USB debugging on, run
     `adb shell appops set com.zeekret.mapslink ACCESS_RESTRICTED_SETTINGS allow`
3. **Type the car's pair code:** in the car open **Zeekret Dash ▸ Map link** (the side menu) and read the 8-letter
   pair code under **Link with the phone app** (like `ABCD-EFGH`). Type it into Zeekret Maps Link, with or without
   the dash; it shows **✓ Valid**.
4. Tap **Open notification access** and turn on **Zeekret Maps Link**.
5. On Xiaomi and other phones that close background apps: **Settings ▸ Apps ▸ Zeekret Maps Link** ▸ Battery saver
   **No restrictions**, and **Autostart** on.

## Use it

1. Start navigation in **Google Maps** (press **Start**; a route preview isn't enough). Android Auto is fine.
2. Within about 15 seconds the car shows **🔋 xx% 🏁**. The app's status line says **Sent: xx.x km left**.
3. Change the destination or add a stop: the car follows by itself.
4. End navigation: the panel goes away.

On the car (Zeekret Dash ▸ **Map link**) you can also set **Home** (shown as **🔋 xx% 🏠** when you're away), pick
a **saved place**, choose the battery size (75 or 100 kWh), and where the panel shows (head unit, HUD or both). On the
head unit, drag the panel to move it and pinch to resize it; it hides in **P**.

## Troubleshooting

| You see | Do this |
|---|---|
| Status stays **Waiting for Google Maps navigation** | Make sure Google Maps is really navigating (its notification with the next turn is showing). Turn notification access off and on again. Check battery saver / autostart (step 5). |
| Status says **Sent** but the car shows nothing | Check the pair code matches the car's. The panel hides in P and while a camera picture is showing. |
| **No connection** | The phone or the car has no internet. |
| Notification access is greyed out | Step 2 above. |

## Questions and problems

Open an [issue](../../issues) with what happened and the app versions.

---

© Zeekret Maps Link. All rights reserved. This repository contains documentation only; no source code is published.
