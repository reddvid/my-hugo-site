---
title: "More Work Automation by Building a WPF App"
description: I built a WPF app with a bunch of tools to automate my workflow
date: 2023-02-02T19:30:40+08:00
featuredimage: "/images/og/work-automation.png"
ogimage: "/images/og/work-automation.png"
hidden: false
comments: true
draft: false
tags: ["Apps", "WPF", "C#", "Visual Studio", ".NET", "Coding", "MySQL", "Regular Expressions", "Fluent", "Software Development", "Automation", "Workflow"]
categories: ["Coding", "Software Development"]
---
# More Work Automation by Building a WPF App

As I mentioned in my previous blog post, I do some online broadcasting/content duties which include posting on the website, monitoring and logging 12 stations’ social media and audio servers, and any particular aspect of it.

Having to monitor the 12 stations’ analytics and statistics, one will always come to the point where they would need to automate their workflow. And that’s what I did.

## Counting WordPress Posts

First up is to check which stations have posted content on the news website. Since I also have the access to cPanel, thus I can whitelist the office’s IP address in phpMyAdmin to access the MySQL database then.

The app connects to the WordPress database, checks if a station has any existing posts on a specific date, then builds a *calendar-like* layout, as shown below:


{{< figure src="posts-activity-page.png" >}}

I know a little bit of SQL querying but with how WordPress database works, it took me a while to learn about _joining_.

I wish I could simplify the query below:

```sql
"SELECT p.ID, p.post_date, p.post_title, t.term_id " +
"FROM wpku_posts p " +
"LEFT JOIN wpku_term_relationships rel ON rel.object_id = p.ID " +
"LEFT JOIN wpku_term_taxonomy tax ON tax.term_taxonomy_id = rel.term_taxonomy_id " +
"LEFT JOIN wpku_terms t ON t.term_id = tax.term_id " +
"WHERE p.post_type = 'post' " +
"AND p.post_status = 'publish' " +
"AND t.term_id NOT LIKE '14' " +
"AND (p.post_date LIKE '%" + NOW_YEAR + "%' OR p.post_date LIKE '%" + LAST_YEAR + "%') " +
"ORDER BY `p`.`post_date` DESC";
```

The query will then be created into a PostActivity object which requires the StationName, Month, Day, and Year properties.

```csharp
while (dataReader.Read())
{
    PostActivityData.Add(new PostActivity()
    {
        StationName = Convert.ToInt32(dataReader["term_id"].ToString()),
        Month = DateTime.Parse(dataReader["post_date"].ToString()).Month,
        Day = DateTime.Parse(dataReader["post_date"].ToString()).Day,
        Year = DateTime.Parse(dataReader["post_date"].ToString()).Year
    });
}
```

Finally, it is now time to display the data into a Calendar for viewing. The view will depend on the selected station from a dropdown (key). 

```csharp
private async Task DisplayActivityCalendar(int key, int year)
{
	string layoutBuilder = Environment.NewLine;
	int week = 0;

	if (PostsActivityData == null && (PostsActivityData?.Count) == 0)
	{
		_ = await new ContentDialog()
		{
			Title = "Error Displaying Posts Activity",
			Content = "SQL data not loaded properly",
			CloseButtonText = "OK",
		}
		.ShowAsync();

		return;
	}

	var stationData = PostsActivityData.Where(s => s.StationName == key);
	var panelOne = new StackPanel() { Orientation = Orientation.Horizontal, Margin = new Thickness(0, 0, 0, 20) };
	var panelTwo = new StackPanel() { Orientation = Orientation.Horizontal, Margin = new Thickness(0, 0, 0, 20) };
	var panelThree = new StackPanel() { Orientation = Orientation.Horizontal, Margin = new Thickness(0, 0, 0, 20) };
	
	for (int month = 1; month <= 12; month++)
	{
		layoutBuilder = string.Empty;
		layoutBuilder += $"{StringHelper.ShowMonthName(month)} ({stationData.Where(x => x.Month == month && x.Year == year).Count()} posts) {Environment.NewLine}";

		week = 0;

		for (int day = 1; day <= DateTime.DaysInMonth(year, month); day++)
		{
			int space = (int)DateTime.Parse($"{year} {month} {day}").DayOfWeek;
			week++;

			if (space > 0 && day == 1)
			{
				week += space;
				layoutBuilder += StringHelper.GenerateSpaces(space);
			}

			layoutBuilder += stationData.Any(x => x.Month == month && x.Day == day && x.Year == year) ? "✅" : "✖️";

			if (week % 7 == 0)
			{
				layoutBuilder += Environment.NewLine;
			}
		}

		var tb = new TextBlock()
		{
			Text = layoutBuilder,
			Margin = new Thickness(0, 0, 28, 0),
			FontFamily = new FontFamily("Segoe UI"),
		};

		if (month <= 4)
		{
			panelOne.Children.Add(tb);
		} 
		else if (month <= 8)
		{
			panelTwo.Children.Add(tb);
		}
		else if (month <= 12)
		{
			panelThree.Children.Add(tb);
		}
	}

	ContentRoot.Children.Add(panelOne);
	ContentRoot.Children.Add(panelTwo);
	ContentRoot.Children.Add(panelThree);
}
```

The *layoutBuilder* is a cheeky code I played around to craft a calendar with a check or “x” mark whether a post exists on that date.

The ContentRoot is a StackPanel in XAML and will hold the three new StackPanels created from the code above.

## Social Media Numbers

This next one really needs an automation tool. This tool will log the 12 stations’ Facebook Pages, Twitter, and YouTube numbers (likes, followers, and channel subscribers).

{{< figure src="social-media-page.png" >}}

### Facebook

Too bad I can’t get around an API for Facebook Pages to get the number of likes and followers so I ended up with web scraping, which I posted [here](https://reddavid.me/scraping-facebook-pages-to-get-number-of-likes-and-followers/).

### Twitter

I used Twitter’s API to get the accounts’ number of followers, although there is a step which I still have to get the account’s ID from the username:

```csharp
string jsonData;

try
{
    var client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", Settings.Default.TwitterBearerToken);
    var response = await client.GetAsync($"https://api.twitter.com/2/users/by/username/{twitterUsername}");
    var responseContent = response.Content;

    /// Convert username (Screen Name) to Twitter Id First
    /// since the API for getting followers
    /// requires the Id instead of the username
    using (var dataReader = new StreamReader(await responseContent.ReadAsStreamAsync()))
    {
        string json = await dataReader.ReadToEndAsync();

        var data = JsonConvert.DeserializeObject<TwitterData>(json).Data;
        response = await client.GetAsync($"https://api.twitter.com/2/users/{data.Id}/followers");
        responseContent = response.Content;

        using (var datumReader = new StreamReader(await responseContent.ReadAsStreamAsync()))
        {
            jsonData = await datumReader.ReadToEndAsync();
        }

        if (jsonData.Contains("Too Many"))
        {
            Debug.WriteLine("API Request too many requests");
            return "-";
        }
    }

    var followers = JsonConvert.DeserializeObject<TwitterDatum>(jsonData).Meta.Result_count;
    return followers.ToString();
}
catch (Exception ex)
{
    Debug.WriteLine(ex.Message);
    return "-";
}
```

I used json2csharp to convert Twitter response to TwitterData object.

### YouTube

As for YouTube, we also get to use an API which will require an API key from Google. And I simply scraped off the response to get the number of subscribers instead of using an object.

```csharp
try
{
    var uri = $"https://www.googleapis.com/youtube/v3/channels?part=statistics&id={channelId}&key={Settings.Default.YouTubeApiKey}";
    var request = (HttpWebRequest)WebRequest.Create(uri);
    var response = (HttpWebResponse)request.GetResponse();
    var responseStream = response.GetResponseStream();

    string subscribers;
    using (var reader = new StreamReader(responseStream))
    {
        string content = await reader.ReadToEndAsync();
        subscribers = Regex.Match(content, @"subscriberCount"":.""(\d+)").Groups[1].Value;
    }

    // Format to add thousands separator
    return String.Format("{0:n0}", double.Parse(subscribers));
}
catch (Exception ex)
{
    Debug.WriteLine(ex.Message);
    return "-";
}
```

Then the numbers are placed into an object to then be *databound* and listed into a DataGrid in XAML.

## CentovaCast Audio Streams

This third tool lists the stations’ current status of their audio server whether it is off or on, their stream status - online or offline, and current listeners.

{{< figure src="servers-page.png" >}}


Getting the server data using CentovaCast API and cast to an object and can be easily *databound* to a DataGrid.

```csharp
string json;
			
var client = new HttpClient();
var response = await client.GetAsync("http://" + url + "/api.php?xm=server.getstatus&f=json&a[username]=" + user + "&a[password]=" + Settings.Default.CentovaPassword); // Don't add port, instead change the username to stations' username (eg. dxdx)
var responseContent = response.Content;
using (var reader = new StreamReader(await responseContent.ReadAsStreamAsync()))
{
	json = await reader.ReadToEndAsync();
}

var stats = JsonConvert.DeserializeObject<StationStat>(json);

return new string[]
{
	stats.Response.Data.Status.Listenercount.ToString(),
	stats.Response.Data.Status.Serverstate.ToString(),
	stats.Response.Data.Status.Bitrate + " kbps",
	"Source IP: " + stats.Response.Data.Status.Sourceip,
	stats.Response.Data.Status.Sourceconnected.ToString()
};
```

The DataGrid also includes a context menu to quickly control the selected station’s server.

```csharp
await client.GetAsync("http://"+ url + "/api.php?xm=server." + command + "&f=json&a[username]=" + radioStation.CallSign.ToLower() + "&a[password]=" + Settings.Default.CentovaPassword);
```

*Commands: restart, reload, stop. The Stream menu item will navigate to the stream page.*

## Text Case Changer

We now go back to WordPress content. I sometimes post news content to the website, and some news write-ups don’t follow the standards - like the title in all-caps, in Unicode characters, etc.

Prior to this, I tried writing my own logic but I was not satisfied until I used DevToys code.

{{< figure src="text-case-changer-page.png" >}}

Prior to this, my logic includes a list of exception words - which will ignore any case change or follow a certain case - like proper nouns.

[DevToys is open source in GitHub.](https://github.com/veler/DevToys)

## URL Shortener

There was this Yourls app in cPanel/Softaculous that allows me to create short URLs but it stopped working one day. And WordPress already has one.

This tool should be renamed as URL Styler, which just styles *accepted* URL input into a custom Viber *message* - we repost news to our Viber community members.

{{< figure src="url-shortener-page.png" >}}

The Customize button is enabled/disabled through ViewModel if the URL is a vaild rpnradio.com or wp.me shortlink.

```csharp
Regex.IsMatch(_inputUrl, @"^http(s)?://([rpnradio|wp]+.)+[com|me-]+(/[\w- ./?%&=]+)?(/)?$")
```

Next is we’ll use the HtmlDecode and RegEx capturing to get the details from the link’s page source (e.g. title, featured image URL, and shortlink if the input is not one).

```csharp
...

string pageContent = await page.GetContentAsync();

var title = HttpUtility.HtmlDecode(Regex.Match(pageContent, @"<title>(.+)</title>").Groups[1].Value.Replace("- Radio Philippines Network", string.Empty).Trim());
var shortLink = Regex.Match(pageContent, @"<link rel=""shortlink"" href=""(https\:\/\/wp\.me\/.+)"">").Groups[1].Value;
var imagePath = Regex.Match(pageContent, @"og:image"" content=""(.+)""").Groups[1].Value;
```

The tool will finally create a custom text to the Clipboard and ready to be pasted in Viber.

## (WIP) Shortcuts

I used an Elgato Stream Deck before to launch most-used URLs and I am currently working to integrate this to the app.

Unfortunately, the code repo is not publicly available but will try to “hide” some confidential details before making it public.