---
title: "TI10 and Appdates"
date: 2021-10-15T03:53:07+08:00
draft: false
slug: ti10-and-appdates
author: "David"

tags:
  [
    "Apps",
    "Gaming",
    "TI10",
    "DOTA 2",
    "The International",
    "Flutter",
    "Android",
    "iOS",
  ]
categories: ["Gaming", "App Development"]
featuredimage: "/images/og/ti10-appdates.png"
comments: true
ogimage: "/images/og/ti10-appdates.png"
---

Let's start with [The International](https://en.wikipedia.org/wiki/The_International_2021#:~:text=The%20International%202021%2C%20also%20known,2%20world%20championship%20esports%20tournament.), shortened as TI, Valve's yearly Dota 2 tournament where the 18 of the best teams around the world gather to fight for the Aegis of the Champions every year. After being canceled and waiting for more than 2 years it is now happening, without an audience in Bucharest, Romania with a total prizepool of $40M+.

<center>
{{< tweet 1444037357775179779 >}}
</center>

I only play the game against custom BOT scripts 🤣 just to past time or just to be updated with what patches <a class="link" href="https://dota2.fandom.com/wiki/IceFrog" target="_blank">IceFrog</a> brings to the game. What I mostly do is watch major tournaments especially those when teams need to earn <a class="link" href="https://liquipedia.net/dota2/Dota_Pro_Circuit" target="_blank">DPC points</a> for entering the prestigious TI.

## 10 Days of DOTA

#TI10 started on October 7th with 18 qualified teams playing playoffs on the group stage to determine their position either on the upper or lower bracket; and where two teams: Thunder Predator and SG, in the end, bid farewell for the Main Stage and championship.

<center>
{{< tweet 1447199723308269568 >}}
</center>

Now, as of this writing, we just ended Day 3 of the Main Stage and I've followed and watched most of the games. Fnatic, a SEA team with 2 Pinoy players, got eliminated on Day 2 just after getting past a heart-wrenching Best-of-One's on the Lower Bracket the day before. The last SEA representative, T1, would have to give their best on their lower bracket run.

{{ < figure src="https://pbs.twimg.com/media/FBrySN5VIAAtGqh?format=jpg&name=large" > }}

Thanks to [Wykrhm Reddy (@wykrhm)](https://twitter.com/wykrhm) for being such a good Dota Community Man.

## The Secret

Watching all games and supporting SEA teams, my heart still goes to Team Secret for all their TI runs. Right now, they already secured Top 3 finish for the semifinals game against PSG.LGD.

<center>
{{< tweet 1447192871677857796 >}}
</center>

I follow the games by watching <a class="link" href="https://www.twitch.tv/singsing" target="_blank">singsing's Twitch stream</a> as he and his (pepega) friends cast and analyze the stream - chill and fun. This way I can watch (or listen) while doing app coding.

## Coding and Appdates

This 10-day tournament meant a break from me streaming (to catch the games) which then allowed me time to code for RPN News mobile app.

My job (finally) provided me a Samsung A71 Android phone to test the app (and for work-related stuff) after my old personal phone went bust; though I still use my iPhone 11 to test the iOS code - maybe soon? 🤣

{{< figure src="/images/10-21/appdates/samsung-a71.png" height="400px" caption="Samsung A71" >}}

The app has been up on the Google Play Store for some time (as I blogged <a class="link" href="https://reddavid.me/rpn-android-app/" target="_blank">here</a>) and lacked significant updates. It stayed that way for a while until all busy-ness and code-confusion (is this even a term?) has subsided.

These past days, I managed to release an update for the Android beta app and also started a TestFlight for iOS devices.

I decided to version the app as 2.0 as this was a complete rework from the old code despite not publicly releasing a version 1.0. The app was previously coded on C#/Xamarin Forms but the documentation and support I needed was "not there", then I started to code on Dart/Flutter for RPN Radio which made things easier - being a single-codebase mobile app development.

Version 2.0 brings a new UI and supports offline mode - see downloaded news (as opposed to before that it will not work when there is no internet; though it is always recommended to be connected to get the latest news).

![News Items Page](news-screen1.png)![News Detail Page](news-screen2.png)

### Get into the Beta Test

The app is available for testing and I welcome your feedback after getting the app here from **[Google Play Store](https://bit.ly/2YVpJC9)** and **[Apple TestFlight](https://apple.co/2XaMBNl)**.

Or scan the QR code/s below:

{{< figure src="/images/10-21/appdates/play-qr.png" alt="Google Play Store QR Code" caption="Google Play Store QR Code" >}}
{{< figure src="/images/10-21/appdates/testflight-qr.png" alt="Google Play Store QR Code" caption="Google Play Store QR Code" >}}

You may submit your feedback using built-in tools from Google or TestFlight, sending an [email](mailto:info@rpnradio.com), or any on my socials.

I will try my best to get the app working for a public release.

---

That's it for now folks, **let's watch and enjoy some Dota**.
