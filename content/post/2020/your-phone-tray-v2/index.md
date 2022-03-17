---
title: "Your Phone Tray V2"
date: 2020-07-08T21:18:31+08:00
draft: false
author: "David"

tags: ["App Development", "Windows 10", "UWP", "Fluent Design"]
categories: ["Coding"]
featuredimage: "/images/feat/your-phone.jpg"
comments: true
ogimage: "/images/og/your-phone-tray.png"
---

Your Phone Tray UWP app just got updated.

Version 2.0 brings few UI changes, some improvements and a breaking change.

## Improvements

### New Context Menu

To improve the app, I updated the tray icon context menu to show that it should be running on Startup (see Task Manager Start up)
{{< figure src="/images/07-20/yourphonetray/ypt-contextmenu.png" caption="Your Phone Tray Context Menu" alt="Context Menu of Your Phone Tray app" width="50%" >}}

#### Shortcut Key

You can also see a new menu item **Enable Ctrl+Y Hotkey**. If enabled, the app silently picks up the `Ctrl+Y` shortcut to open Your Phone app. It can be toggled if the shortcut key messes up some of your programs.

<hr>

## Breaking change

Microsoft's Your Phone app has been updated several times that launching it directly to the photos section shows a blank UI. For the meantime, all shortcuts to open the app will direct to messages.

## Download

<a class="link" href="https://bit.ly/UrPhoneTray" target="_blank">**Download the app here**</a> for Windows 10
