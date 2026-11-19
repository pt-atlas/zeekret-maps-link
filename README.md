# Zeekret Maps Link (phone)

A small Android phone app for the **Zeekr 7X**. While **Google Maps** navigates on your phone (also through
**Android Auto**), it sends the **distance left on your route** and the **arrival time** to **Zeekret Dash** on the
car's head unit. Zeekret Dash then shows the **battery % you'll arrive with**, right on the car's screen:

**🔋 69% 🏁**

Change the destination in Google Maps and the car follows within seconds. No sharing, no typing on every trip.

**New in 1.2:** tap a charger in Zeekret Dash's **Nearby Charger** suggestions on the car, and it opens in Google Maps on
your phone (and so on Android Auto), ready to navigate. Needs **Display over other apps** (step 7).

> This repository holds **documentation only, no source code**. Zeekret Maps Link is closed-source.

> 🚗 **Tested with the Zeekr 7X only (SEA region).** Any other car or region is entirely at your own risk.

> 🧪 **Free, unofficial, personal project.** Not affiliated with Zeekr, Geely, ECARX or Google. No warranty and no
> responsibility for anything that happens from using it. The % is an estimate: always keep an eye on the car's own
> range.

> 🔒 **A Zeekr owner's private project, on a private server. No personal data is collected** — no location, no
> account, no driving data, no analytics. The only link is the car's pair code: the phone sends a locked box (km left, arrival time)
> under that code, and the server can't open it. Old boxes are deleted after 24 h.

## The Zeekret apps

| App | Runs on | What it does |
|---|---|---|
| **[Zeekret](https://github.com/pt-atlas/zeekret-updates)** | Phone + Wear OS watch | Bluetooth digital key, remote control, stats, widget |
| **[Zeekret Dash](https://github.com/pt-atlas/zeekret-dash)** | Car head unit | Blindspot camera, mirror, dash cam, car controls, **battery at arrival** |
| **Zeekret Maps Link** (this one) | Phone | Sends your live Google Maps route to Zeekret Dash. Works without the Zeekret app; needs Zeekret Dash (pair code) |

**Works on its own: you don't need the Zeekret phone app.** You only need **Zeekret Dash** in the car, linked with its
**pair code** (Zeekret Dash ▸ side menu ▸ **Map link**). Without Zeekret Dash, Zeekret Maps Link shows nothing.

**🌐 All three apps on one page:** **[zeekret.pages.dev](https://zeekret.pages.dev/)**, in two parts: **📱 Phone apps** (Zeekret, Zeekret Maps Link; downloads from the latest GitHub release) and **🚗 Zeekr 7X side app** (Zeekret Dash, opened on the car's screen).

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

## Install and set up

**⬇ Download:** [latest release](https://github.com/pt-atlas/zeekret-maps-link/releases/latest) →
`ZeekretMapsLink-1.2.apk`.

### Why setup needs a few extra steps

Zeekret Maps Link has to read **Google Maps' navigation notification**, so it needs **notification access**.
Android treats notification access as a **restricted setting** for apps that don't come from the Play Store: the
switch is **greyed out** until you allow it. On some phones **Google Play Protect** also refuses to install such apps.
The steps below get around both, safely, on your own phone.

You need:
- your Android phone and its USB cable;
- **once**, a computer (Windows, Mac or Linux) — only for step 2 and 4;
- the car's **pair code** from Zeekret Dash (step 5).

### Step 1 — Turn on debug mode (USB debugging) on the phone

1. **Settings ▸ About phone**, then tap **Build number** 7 times until it says *"You are now a developer"*.
   - Xiaomi / Redmi / POCO: **Settings ▸ About phone ▸** tap **OS version** (or **MIUI version**) 7 times.
   - Samsung: **Settings ▸ About phone ▸ Software information ▸** tap **Build number** 7 times.
2. Go back to **Settings ▸ System ▸ Developer options** (Xiaomi: **Settings ▸ Additional settings ▸ Developer
   options**) and turn on **USB debugging**.
   - Xiaomi only: also turn on **Install via USB** (it may ask you to sign in to a Mi account).

### Step 2 — Get the `adb` tool on the computer

1. Download Google's **SDK Platform-Tools** for your computer:
   [developer.android.com/tools/releases/platform-tools](https://developer.android.com/tools/releases/platform-tools).
2. Unzip it. You get a folder called **platform-tools**.
3. Put `ZeekretMapsLink-1.2.apk` **into that platform-tools folder**.
4. Open a terminal **in that folder**:
   - Windows: open the folder, click the address bar, type `cmd` and press Enter.
   - Mac / Linux: open Terminal and type `cd ` (with a space), drag the folder in, press Enter.
5. Plug the phone into the computer. On the phone, tap **Allow** on *"Allow USB debugging?"* (tick *Always allow
   from this computer*).
6. Check the phone is seen — type this and press Enter:

   ```
   adb devices
   ```

   You should see one line ending in **device**. If it says **unauthorized**, look at the phone and tap **Allow**.
   (Mac / Linux: type `./adb` instead of `adb` in every command.)

### Step 3 — Install the app

```
adb install -r ZeekretMapsLink-1.2.apk
```

It prints **Success**. (If you could install the APK by tapping it on the phone, you can skip this command.)

### Step 4 — Allow the restricted setting

```
adb shell appops set com.zeekret.mapslink ACCESS_RESTRICTED_SETTINGS allow
```

It prints **nothing** when it worked. Type it exactly: the app name is `com.zeekret.mapslink` — **maps** with an
**s**. *"No UID for …"* means a typo.

> **No computer?** On many phones (not Xiaomi) you can do steps 3–4 on the phone instead: tap the APK to install it,
> try step 6 once (the switch is greyed out), then go to **Settings ▸ Apps ▸ Zeekret Maps Link ▸ ⋮ (top right) ▸
> Allow restricted settings**. If you don't see that menu, use the computer.

You can unplug the phone now and turn **USB debugging** off again if you like. It's only needed once.

### Step 5 — Type the car's pair code

1. In the car, open **Zeekret Dash ▸ Map link** (side menu). Under **Link with the phone app** you see the pair
   code, like `ABCD-EFGH`.
2. Open **Zeekret Maps Link** on the phone and type that code (with or without the dash). It shows **✓ Valid**.

### Step 6 — Allow notification access

1. In Zeekret Maps Link tap **Open notification access**.
   (Or: **Settings ▸ Apps ▸ Special app access ▸ Notification access**; on Xiaomi **Settings ▸ Notifications ▸
   Notification access**, or search the settings for "notification access".)
2. Find **Zeekret Maps Link** in the list and switch it **on**.
3. Android warns that the app can read notifications: tap **Allow**. (It only ever reads Google Maps' navigation
   notification, and only the distance and arrival time — see above.)
4. Back in Zeekret Maps Link, both lines show **✓**: *Valid* and *Allowed*.

### Step 7 — Allow display over other apps (for chargers, 1.2+)

In Zeekret Maps Link tap **Open display over other apps** and switch **Zeekret Maps Link** on. Android only lets an app
open Google Maps from the background with this. The line shows **✓ Allowed**. It only checks for a tapped charger
while Google Maps / Android Auto navigation is running.

### Step 8 — Keep it running (Xiaomi and phones that close background apps)

**Settings ▸ Apps ▸ Zeekret Maps Link ▸ Battery saver ▸ No restrictions**, and turn **Autostart** on.

## Use it

1. Start navigation in **Google Maps** (press **Start**; a route preview isn't enough). Android Auto is fine.
2. Within about 15 seconds the car shows **🔋 xx% 🏁**. The app's status line says **Sent: xx.x km left**.
3. Change the destination or add a stop: the car follows by itself.
4. End navigation: the panel goes away.
5. Tap a charger in the car's **Nearby Charger** suggestions: within a few seconds Google Maps on the phone opens it
   (while navigation is running).

On the car (Zeekret Dash ▸ **Map link**) you can also set **Home** (shown as **🔋 xx% 🏠** when you're away), pick
a **saved place**, choose the battery size (75 or 100 kWh), and where the panel shows (head unit, HUD or both). On the
head unit, drag the panel to move it and pinch to resize it; it hides in **P**.

## Troubleshooting

| You see | Do this |
|---|---|
| Status stays **Waiting for Google Maps navigation** | Make sure Google Maps is really navigating (its notification with the next turn is showing). Turn notification access off and on again. Check battery saver / autostart (step 8). |
| Status says **Sent** but the car shows nothing | Check the pair code matches the car's. The panel hides in P and while a camera picture is showing. |
| Tapping a charger in the car does nothing | Step 7 (display over other apps). Google Maps navigation must be running on the phone. Same pair code on both. |
| **No connection** | The phone or the car has no internet. |
| Notification access is greyed out | Step 4 (the restricted setting) wasn't done, or had a typo. |
| `adb` says **unauthorized** / **no devices** | Unlock the phone and tap **Allow** on the USB debugging message; try another cable or USB port. |
| `adb` is "not recognized" | The terminal isn't open in the platform-tools folder (step 2.4). |

## Questions and problems

Open an [issue](../../issues) with what happened and the app versions.

---

© Zeekret Maps Link. All rights reserved. This repository contains documentation only; no source code is published.
