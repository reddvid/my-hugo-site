---
title: "[Update: NOW AVAILABLE] ZIP Code PH for iOS"
date: 2021-03-16T01:28:15+08:00
draft: false
description: "Download ZIP Code PH now for iOS"
author: "David"
slug: zipcodeph-ios
tags: ["iOS", "Coding", "SwiftUI", "Apple"]
categories: ["Coding"]
featuredimage: "/images/feat/zipcodeph-ios.png"
comments: true
lightgallery: true
ogimage: "/images/og/zipcodeph-ios.png"
---

## Now Available

You can get the app from App Store (iPhone and iPad) for only Php49 (US$0.99) - sorry for the paywall. I'm still a struggling developer and just to let you know, developing for Apple costs more compared to Windows and Google Play Store.

Get the app below:

<center>
<a class="link" href="https://apps.apple.com/us/app/zip-code-ph/id1555921863?itsct=apps_box&amp;itscg=30200" style="display: inline-block; overflow: hidden; border-radius: 13px; width: 250px; height: 83px;"><img src="https://tools.applemediaservices.com/api/badges/download-on-the-app-store/black/en-us?size=250x83&amp;releaseDate=1615766400&h=60d62bace1ec567408893f9085310f3c" alt="Download on the App Store" style="border-radius: 13px; width: 250px; height: 83px;"></a>
<br><img src="https://tools-qr-production.s3.amazonaws.com/output/apple-toolbox/a1b53b4233734ce37673d9f70803db74/47cd37c9-fb35-4a15-b8b8-958219968ea0.png" width="200px">
</center>
<hr>

## Let's Go

For the last few weeks, I spent my free time building my app, ZIP Code PH, for iOS using Apple's native language SwiftUI.

Using a Mac is not that hard anymore since I've test ran macOS Catalina on VirtualBox. I managed to run XCode and tried to tinker with the language, and built some UI mockups.

And luckily, the workplace has acquired a mac mini to cater Apple users for our apps (which is coming as soon as possible). I will also be the one developing the iOS apps, so check them out soon.

## Development

### Main and About Page

I am fairly new to SwiftUI so converting? the codes I used for Windows and Xamarin Android app - XAML and C# - will be useless. It took a few days to build the bare UI. I started with the Main Page and the Help page since they don't consume a lot of code-behind.

The Main Page took a lot of iterations but below is the final view (as of this writing):

{{< figure src="/images/03-21/zipcodeph-ios/mainpage.png" width="40%" caption="Main Page" alt="Main Page" >}}

The Help/About Page contains the same details I used in the Android app except that I added attributions to the new images in the Main Page (thanks <a class="link" href="https://unsplash.com/" target="_blank">Unsplash!</a>)

<figure class="image">
<img src="/images/03-21/zipcodeph-ios/about-1.png" style="display:inline;margin-left:auto;margin-right:auto;width:40%;">
<img src="/images/03-21/zipcodeph-ios/about-2.png" style="display:inline;margin-left:auto;margin-right:auto;width:40%;">
<figcaption>Help & About Page</figcaption>
</figure>
<br/>

### Search Page

Next up is the Search Page, I coded this before the menus since the code-behind logic is much simpler since we will use all the zip codes instead of filtering them to areas.

I didn't explorer SwiftUI that much but I found this code to build the Search bar iOS-style:

{{< figure src="/images/03-21/zipcodeph-ios/searchbar.png" width="60%" caption="iOS Search Bar" alt="iOS Search Bar" >}}

```swift
HStack {
    TextField("Search ZIP Code PH", text: $text)
        .padding(7)
        .padding(.horizontal, 25)
        .background(Color(.systemGray6))
        .cornerRadius(8)
        .overlay(
            HStack {
                Image(systemName: "magnifyingglass")
                    .foregroundColor(.gray)
                    .frame(minWidth: 0, maxWidth: .infinity, alignment: .leading)
                    .padding(.leading, 8)

                if isEditing && !self.text.isEmpty {
                    Button(action: {
                        self.text = ""
                    }) {
                        Image(systemName: "multiply.circle.fill")
                            .foregroundColor(.gray)
                            .padding(.trailing, 8)
                    }
                }
            }
        )
        .padding(.horizontal, 20)
        .onTapGesture {
            self.isEditing = true
        }

    if isEditing {
        Button(action: {
            print(self.$text)
            self.isEditing = false
            self.text = ""
            // Dismiss the keyboard
            UIApplication.shared.sendAction(#selector(UIResponder.resignFirstResponder), to: nil, from: nil, for: nil)
        }) {
            Text("Cancel")
        }
        .padding(.leading, -10)
        .padding(.trailing, 10)
        .transition(.move(edge: .trailing))
        .animation(.easeInOut)
    }
}
```

<br>
<video muted width="280" controls style="display:block;margin-left:auto;margin-right:auto;">
  <source src="/videos/03-21/zipcodeph-ios/searchpage.mp4" type="video/mp4">
</video>

I spent a few times making the search (filtering) logic work the way I wanted that I almost gave up with SwiftUI programming 😅

## Menu and Area List Page

After understanding the filtering code while developing the Search Page, it is time to code the Menu and Area pages. The UI looks a lot like the Android counterpart but the built-in Title style for iOS makes the UI better by collapsing the title to inline when scrolled. This is how it looks like on version 1.0.4

{{< figure src="/images/03-21/zipcodeph-ios/area.png" width="40%" caption="City and Area Pages" alt="City and Area Pages" >}}

## Favorites

Now the challenge begins, how to implement favorites - saving and loading data - while making the UI flexible when an item is added to favorites.

After more than a week of mind-bending development, I made it to work with the help of stackoverflow 😅. (Every developer use it, right? Right?)

<video muted width="280" controls style="display:block;margin-left:auto;margin-right:auto;">
  <source src="/videos/03-21/zipcodeph-ios/favorite.mp4" type="video/mp4">
</video>
<br/>

## Showcase

Below are the app showcase which you can see on the App Store as soon as the app is available

![Light and Dark Mode](gallery/color-mode.png)
![Favorites Page](gallery/favorites.png)
![Search Page](gallery/search.png)
![Trivias](gallery/trivia.png)

![NCR and Luzon](gallery/ncr-and-luzon.png)
![Visayas](gallery/visayas.png)
![Mindanao](gallery/mindanao.png)

## Downloads

(Now Available!) <a class="link" href="https://apple.co/3lrnrBO" target="_blank">Get the app</a> on App Store for iPhone and iPad.

The app can be tested and I will share the TestFlight link soon. I am still fixing some minor issues and will announce the availability on a new blog post.

For the meantime, the app is still available for Android and Windows 10.

You can check the blog posts here for the download links:
<br/>
<a class="link" href="/zipcodeph-android-dark-mode/">ZIP Code PH for Android gets Dark Mode
</a>
<br/>
<a class="link" href="/zipcodeph-new-ui/">ZIP Code PH Gets Refreshed UI</a>
