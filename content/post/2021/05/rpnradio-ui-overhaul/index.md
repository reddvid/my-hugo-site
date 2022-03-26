---
title: "RPN Radio UI Overhaul"
date: 2021-05-30T00:15:36+08:00
draft: false
author: "David"
slug: rpnradio-ui-overhaul
tags: ["Apps", "RPN", "Apps", "Software Development", "Application Development"]
categories: ["Coding"]
featuredimage: "/images/feat/rpnradio-ui.jpeg"
comments: true
ogimage: "/images/og/rpnradio-newui-og.png"
---

Browsing Instagram on my account now floods me with several UI/UX tips and practices, making me inspired to update and retouch my apps with some latest trends in mobile designs.

## Old Look

Let's take a look at what the app looked like before the update:

<figure class="image">
<img src="/images/02-21/rpnradio/stationlist.png" alt="RPN Radio Stations" style="display: inline; width: 40%;">
<img src="/images/02-21/rpnradio/playingview.png" alt="RPN Radio Streaming" style="display: inline; width: 40%;">
<figcaption>Old UI</figcaption>
</figure>
<br/>

Though it looked intuitive - with just two simple steps: 1) Launch app, 2) Select station and Listen; the main page does not give the user context on what the app does, which raise this question:

> What am I supposed to do?

## Overhaul

{{< figure src="https://media.giphy.com/media/xZsLh7B3KMMyUptD9D/giphy.gif" >}}

### Menu List

At first glance of a first-time user, the stations list does NOT show that it is "clickable". So the first UI update was to change the 4x3 grid to a simple list.

{{< figure src="/images/05-21/rpnradio-newui/menu-list-bna.png" title="Station Menu Item Before and After" width="90%" alt="RPN Radio Stations">}}

This subtle change provides the user a hint of "click me" by giving the list item a design of a button. It also gave a space for the station's detail to be added - station's location and live/offline status.

### Header and Controls

Ok, this area will look crammed especially on small-screen devices. Before this update though, I repositioned the buttons below the controls and were still crammed. This update allowed the horizontal list to be scrolled.

{{< figure src="/images/05-21/rpnradio-newui/head-controls-bna.png" title="Header & Controls Before and After" width="90%" alt="RPN Radio Stations">}}

I also changed the text that shows the current stations into a simpler one and updated the share icon.

### New Font

I like [Poppins](https://fonts.google.com/specimen/Poppins).
<br>

### About

Besides the UI updates mentioned above, I added an "About" page. It is a simple about and external links of RPN.

{{< figure src="/images/05-21/rpnradio-newui/rpnradio-about.png" title="About page" width="60%" alt="RPN Radio About">}}

## Roadmap

These are my to-do list for the app, which we will see soon on future updates:

- No internet connection status
- Bottom player controls (Priority)
- App-aware sharing
- Rate and review prompt
- Links to station email/messaging

## Downloads

As always, the app is still available for Android and really soon for iOS.

[Google Play Store](https://play.google.com/store/apps/details?id=com.rpnradio.radiov1)

[Huawei AppGallery](https://appgallery.huawei.com/#/app/C103076031)
