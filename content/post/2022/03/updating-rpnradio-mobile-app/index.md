---
title: "Updating RPN Radio Mobile App"
slug: "updating-rpnradio-mobile-app"
description: Version 2.0 coming soon
date: 2022-03-31T00:44:04+08:00
featuredimage: "rpnradio-updates-feat.png"
ogimage: "rpnradio-updates-og.png"
hidden: false
comments: true
draft: false
tags:
  [
    "Flutter",
    "Dart",
    "Android",
    "iOS",
    "VS Code",
    "Android Studio",
    "ExpansionTile",
    "Swiper",
  ]
categories: ["Apps", "Coding"]
---

## Update 1

<small>2022-03-31</small>

I just refactored the FutureBuilder by separating the future tasks to where they are actually used in the UI - to prevent long startup and show the main UI at launch instead of a loading screen.

Version 2.0.1 is now on open testing for Android on Google Play Store.

Download link: [RPN Radio for Android (Beta)](https://play.google.com/store/apps/details?id=com.rpnradio.radiov1&hl=en&gl=US)

<hr>

It's been a while since I coded and updated my apps, it just somehow began again when I felt the urge to refresh my website's looks. Now I am aiming to light up every Github contribution grid block whenever I can.

## Updating Flutter Packages

The first thing I did was to check for updated packages from pub.dev by using <code>flutter pub upgrade</code>

Then most are already outdated and forced some to major versions with<code>flutter pub upgrade --major-versions</code>

Also, I tried to use Android Studio for Flutter programming and it took a little while again to get used to keyboard shortcuts coming from VS Code.

Like the usual <code>Alt + Shift + F</code> to format code, now I have to use two hands for <code>Ctrl + Shift + L</code>. Also the shortcut for navigating to implementations <code>F12</code> on VS Code, it's now <code>Ctrl + Alt + B</code>. Heck, I can't be bothered wasting time to remap those shortcut keys...

Despite the hassle above, Flutter coding in Android Studio also has awesome perks like: better debugging, better error checking by highlighting files with errors, faster navigation to files, assets manager and Intellisense. Though Android SDK management is still a pain in the a\*\*. Change my mind.

## New Code

My project files were all over the place, so I finally bothered to refactor some files to proper folders like putting model classes to a <code>model</code> folder, services to <code>services</code> folder, etc.
{{< figure src="img/refactoring-files.png" caption="Refactoring files to folders"  >}}

Upgrading packages with at least 3 versions above since the app's last update brought so much changes. Honestly I followed this open source project **([Flutter Audio Service Demo](https://github.com/suragch/flutter_audio_service_demo))** from GitHub and slightly modified/refactor the codes to minimize and actually use the code my app will only use.

### Futures

I also updated the app with a new feature to fetch sponsors' ad placement images from a URL then view them when they are actually _present_ in the contract - I used [JSON](https://www.json.org/json-en.html) to fetch the data. I faced a problem decoding it when I faced this unfamiliar type <code>Map<String, dynamic></code> coming from a C# perspective.

I managed to get it to work but a new issue appeared, two tasks are being awaited even the UI is now visible which made two variables <code>null</code> when they are still being done, making some unwanted behaviors.

Then I finally used a **_Future Builder,_** it basically waits my tasks to finish fetching data before showing the main UI preventing further actions that require the variables.

```dart
return FutureBuilder(
  future: Future.wait([isLiveList(), getImages()]),
  builder: (context, AsyncSnapshot<List<dynamic>> snapshot) {
  if (snapshot.hasData) {
    final liveList = snapshot.data[0];
    final adsList = snapshot.data[1];
    return MainUI(); // Omitted for brevity
  } else {
    return loadingScreen(); // Omitted for brevity
  }
});
```

### ExpansionList and Swiper

Now I need to add a feature for the app to show a carousel/auto-scrolling of images after being checked if it's on the database - now it's based off a .json file.

As stated above, I don't know if Flutter/Dart preferred indexing a <code>List</code> by using a <code>Map</code> and not a number-index. So I poorly crammed the codes online to fetch the json, decode to objects, index, and list the image names.

```dart
Future<Map<String, dynamic>> getImages() async {
  Map<String, dynamic> values;
  final response = await http.get(Uri.parse("https://...ads.json"));
  if (response.statusCode == 200) {
    // If the call to the server was successful, parse the JSON
    values = json.decode(response.body);
    return values;
  }
}
```

Before adding the view though, I thought heavily on where to put it avoiding the user to a second page. Somehow this is what I went for:

{{< figure src="img/sponsors-control.png" caption="Sponsors View using ExpansionTile and Swiper" width="40%" >}}

Sponsors View using ExpansionTile and Swiper

I used <code>ExpansionTile</code> to allow the users to collapse/expand the view and <code>Swiper</code> to show an auto-scroll of the images.

Here's the code for the Sponsors UI:

```dart
ExpansionTile(
  title: Text(
    'Sponsors',
  ),
  textColor: Colors.white,
  collapsedTextColor:
    Colors.white,
  iconColor: Colors.white,
  children: [
    Padding(
      padding:
        EdgeInsets.only(bottom: 20.0),
          child:
            ConstrainedBox(
                constraints: new BoxConstraints.loose(
                   new Size(MediaQuery.of(context).size.width, 280.0)),
                child:
                new Swiper(
                  autoplay: true,
                  autoplayDelay: 10000,
                  itemBuilder: (BuildContext context, int index) {
                    return CachedNetworkImage(
                      imageUrl: imageUrl,
                      fit: BoxFit.fitHeight,
                      width: MediaQuery.of(context).size.width * 0.60,
                    );
                  },
                  itemCount: adsList[stationNames[_nowPlayingIndex]].length,
                  pagination: new SwiperPagination(),
    ))),
  ],
)
```

## To-Do

It's hard work but I would look to further clean and optimize the code.

- Do not use Swiper when image list is only 1
- Improve loading screen during FutureBuilder
- Handle “offline” connection
