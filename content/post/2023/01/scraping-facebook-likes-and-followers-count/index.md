---
title: "Scraping Facebook Pages to Get Number of Likes and Followers"
description:
date: 2023-01-23T02:58:40+08:00
featuredimage: "/images/og/scraping.png"
ogimage: "/images/og/scraping.png"
hidden: false
comments: true
draft: false
tags: ["Regex", "Scraping", "PuppeteerSharp", "Facebook", "C#"]
categories: ["Coding"]
---

Ever since Facebook’s New Page Experience update, my app that logs several Facebook pages’ number of likes and followers have been broken.

Prior to that, I was using a simple HttpClient to receive a page response content and scrape off content (HTML element) that holds the values for the likes and followers, respectively, using Nodes() from HtmlDocument().

Everything works just fine then the update happened.

## Old Way

Before getting into my _working_ method, let me show you the old code (redacted as much as possible):

```csharp
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("https://facebook.com/pg/" + fbId + "/community");
HtmlDocument htmlDoc = new HtmlDocument();

using (var response = (HttpWebResponse)(request.GetResponse()))
{
    HttpStatusCode code = response.StatusCode;
    if (code == HttpStatusCode.OK)
    {
        using (StreamReader sr = new StreamReader(response.GetResponseStream(), Encoding.UTF8))
        {
            htmlDoc = new HtmlDocument();
            htmlDoc.OptionFixNestedTags = true;
            htmlDoc.Load(sr);
        }
    }
}

string likesXPath;
string followersXPath;

// This XPath code changes without prior notice
// so manually updating the code... everytime it shows empty
// and sometimes it's not unique. So...
var likes = htmlDoc.DocumentNode.SelectNodes("//div[@class='_59k _2rgt _1j-f _2rgt']")[5].InnerText;
var followers = htmlDoc.DocumentNode.SelectNodes("//div[@class='_59k _2rgt _1j-f _2rgt']")[7].InnerText;
```

As seen on the comment above, the HTML XPath code changes sometimes, and I also faced where the XPath isn’t unique for the twelve pages I log - for monthly statistics.

And besides the New Page Experience update, not all 12 pages got it. So I scoured thru _source-inspecting_ and debugging on what differs from the NPE and Classic Pages (as of this writing):

## New Response Source Code

Upon _inspecting_ the http response, I saw particular lines from the Classic page:

**Number of likes**

```json
"global_likers_count":XXX
```

**then number of followers**

```json
"follower_count":XXX
```

And lines from the New Page Experience:

**Number of likes**

```json
"text":"XXX likes"
```

**and number of followers**

```json
"text":"XXX followers"
```

## Regular Expressions

With these unique lines, it is now time to scrape the values out from them. And what a wonderful way to _capture_ them? **Regex**, everyone’s favorite! (Sarcasm)

I tested the regex using [**DevToys** on Windows 11](https://devtoys.app/), and the pattern is as literal ~~simple~~ as possible (it just works!):

Regex Pattern for Classic Pages:

```csharp
@"global_likers_count\""\:(\d+)" // For Likes
@"follower_count\""\:(\d+)" // For Followers
```

and for the New Page Experience:

```csharp
@"(\d+\.?\d*[K]|\d+) likes"
@"(\d+\.?\d*[K]|\d+) followers"
```

The regex pattern for the NPE was a little bit tricky since it changed how the numbers are shown - instead of the full numerical amount (e.g. 48,321), Facebook shortened it to 48.3K.

## Full Scraping Code

I also changed how I get the response code, using **PuppeteerSharp**.

```csharp
 /// <summary>
/// Scrape Facebook Page to get the number of Likes and Followers
/// </summary>
/// <param name="facebookId">The Facebook Page Id</param>
/// <returns>An array of string with the number of likes and followers</returns>
public static async Task<string[]> GetFacebookStats(string facebookId)
{
    string likes;
    string followers;

    try
    {
        var options = new LaunchOptions()
        {
            Headless = true,
            ExecutablePath = "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe"
        };

        var browser = await Puppeteer.LaunchAsync(options, null);
        var page = await browser.NewPageAsync();
        await page.GoToAsync($"https://www.facebook.com/{facebookId}/about");

        /// Get the Page HTML content
        /// and check for specific text whether the Page
        /// is still using Classic or New Page Experience
        /// Classic pages content: {"global_likers_count":XXX},"follower_count":XXX
        /// New Page Experience: "text":"XXX likes" "text:"XXX followers"
        /// And use Regex to capture the numbers
        string pageContent = await page.GetContentAsync();

        if (pageContent.Contains("follower_count"))
        {
            likes = Regex.Match(pageContent, @"global_likers_count\""\:(\d+)").Groups[1].Value;
            followers = Regex.Match(pageContent, @"follower_count\""\:(\d+)").Groups[1].Value;

            likes = String.Format("{0:n0}", double.Parse(likes));
            followers = String.Format("{0:n0}", double.Parse(followers));
        }
        else
        {
            likes = Regex.Match(pageContent, @"(\d+\.?\d*[K]|\d+) likes").Groups[1].Value;
            followers = Regex.Match(pageContent, @"(\d+\.?\d*[K]|\d+) followers").Groups[1].Value;
        }

        return new string[2] { likes, followers };
    }
    catch (Exception ex)
    {
        Debug.WriteLine(ex.Message);
        return new string[2] { default, default };
    }
}
```

Overall, it’s a fun way to do regex and finally get the scraping (logging) to work again.
