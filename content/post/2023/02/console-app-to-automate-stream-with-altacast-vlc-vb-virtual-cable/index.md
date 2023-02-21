---
title: "Console App to Automate Stream with AltaCast, VLC, VB Virtual Cable"
description: Making a console app to automate online radio with AltaCast, VLC, and VB Virtual Audio Cable
date: 2023-02-20T17:03:47+08:00
featuredimage: "/images/og/stream.png"
ogimage: "/images/og/stream.png"
hidden: false
comments: true
draft: false
tags: ["C#", "CSharp", "Software Development", ". NET", "dotNet", "Console app", "AltaCast", "online radio", "VLC", "Programming", "VB Virtual Audio Cable"]
categories: ["Coding"]
---

[02/22 Updated Code to Fix Timer](#programming-a-console-app)

What happens when radio station’s go offline - or after their broadcast hours end? Dead air. Well, I am not sure if it is also the term being used in 24/7 audio servers.

This “dead air” not only affects the usability of streaming apps but also prevents users who differ in time zones. These users will be deprived of any content thus affecting possible audiences for the station.

I recently brought up to the team by letting our stations utilize the audio servers 24/7 by adding program replays during off-air hours. The servers, which use CentovaOS, allow uploading audio files to be played during “off-stream” called AutoDJ. The stations can also let their PCs run 24/7 but the cost, manpower, and effort to implement such system seem to be a struggle as operations differ from each station.

Since I have been letting my work PC “on” all of the time, I proceeded to make it as a client to stream filler programs at times all stations are off-the-air.

## Multi-port Stream with AltaCast

We’ve been using [butt (broadcast using this tool)](https://sourceforge.net/projects/butt/)  and/or [WinAmp Shoutcast DSP](https://directory.shoutcast.com/Winamp) to upload a single stream to the servers (each station) - and running 12 instances of it is highly unrecommended.

My internet search led me to a program called [AltaCast](http://www.altacast.com/) - which supports multiple streams. The standalone app initially supports OGG encoded streams but downloading [MP3Lame](https://lame.sourceforge.io/) encoders now supports MP3 (duh).

I started off streaming one port, and it works well. Then 3, then all 12. But the app crashes. Further troubleshooting let me conclude that it might be limited to 6 simultaneous streams.

Now with that limitation, this thing would be impossible. Then I tried to “copy” the standalone app on another location and reconfigured the remaining 6 streams, and it worked!

The only problem is that there is no way to rename the configurations as to which is which.

![AltaCast](altacast.png)

## Virtual Cable

There should be nothing wrong to use the desktop audio to stream, but to avoid any unwanted noise/audio, I installed [VB Virtual Audio Cable](https://vb-audio.com/Cable/) which installed a virtual audio I/O on Windows to re-route certain apps’ audio.

I used the VB Cable Output as the input for two instances of AltaCast.

## Media Playback

I will be using [VLC](https://www.videolan.org/vlc/) as the media player since it supports network streams. Right now, I will be “hooking-up” to [DWIZ 882](https://www.dwiz882am.com/) (one of our affiliate station) programs upto 12 midnight. Then use [CNN Philippines](https://cnnphilippines.com/) programs 12-3am.

## Programming a Console App

This app will be a timer to check set times:

* 10PM - Start Client Stream
* 12MN - Change Media Source
* 3AM - Stop/Quit Stream

**UPDATE** 
* Fixed various bugs (time check)
* Changed every second tick to every minute
* Adjusted Trim Extension to remove seconds

Below is the full code:

```csharp
using System;
using System.Diagnostics;
using System.Threading;

public class Program
{
    private static Timer? _timer;
    private static bool isActive = false;

    public static void Main()
    {
        Console.WriteLine();
        Console.WriteLine("Welcome to Stream Control!");
        Console.WriteLine("--------------------------");
        Console.WriteLine("App Version: 0.1-beta");
        Console.WriteLine();

        Console.WriteLine("Starting timer...");
        // Changed timer tick to 60_000 ms | 60 secs
        _timer = new Timer(TimerCallback, null, 0, 60_000);

        Console.ReadLine();
    }

    private static void TimerCallback(Object o)
    {
        TimeSpan start = TimeSpan.Parse("22:10");
        TimeSpan shift = TimeSpan.Parse("23:59");
        TimeSpan stop = TimeSpan.Parse("02:55");
        TimeSpan now = DateTime.Now.TimeOfDay;
        now = now.StripSeconds();

        Console.WriteLine($"Now: {now} | Start: {start} | Shift: {shift} | Stop: {stop}");

        if (now == start && !isActive)
        {
            StartLocalStream();
            isActive = true;
        }
        else if (now == shift && isActive)
        {
            // Change Media Playback
            ChangeMediaPlayback();
        }
        else if (now == stop && isActive)
        {
            // Stop apps
            KillProcesses("vlc");
            KillProcesses("altacastStandalone");
            isActive = false;
        }
    }   

    private static void ChangeMediaPlayback()
    {
        // Get media playing for max 3 hours
        // Kill any initial VLC
        var vlc = @"C:\Program Files\VideoLAN\VLC\vlc.exe";

        KillProcesses("vlc");

        CreateAndRunProcess(directory: @"C:\Program Files\VideoLan\VLC", fileName: vlc, args: "https://streaming.cnnphilippines.com/live/myStream/playlist.m3u8");
    }

    private static void StartLocalStream()
    {
        var vlc = @"C:\Program Files\VideoLAN\VLC\vlc.exe";
        var altaCast1 = @"D:\altacast\altacastStandalone.exe";
        var altaCast2 = @"D:\altacast2\altacastStandalone.exe";

        CreateAndRunProcess(directory: @"C:\Program Files\VideoLan\VLC", fileName: vlc, args: "http://149.56.147.197:9079/stream");
        CreateAndRunProcess(directory: @"D:\altacast\", fileName: altaCast1, isShell: true, verb: "runas");
        CreateAndRunProcess(directory: @"D:\altacast2\", fileName: altaCast2, isShell: true, verb: "runas");
    }

    private static void CreateAndRunProcess(string directory, string fileName, string args = null, bool isShell = false, string verb = null)
    {
        ProcessStartInfo procInfo = new ProcessStartInfo
        {
            WorkingDirectory = directory,
            FileName = fileName,
            UseShellExecute = isShell,
            Verb = verb,
            Arguments = args
        };
        Process.Start(procInfo);
    }

    private static void KillProcesses(string processName)
    {
        foreach (var process in Process.GetProcessesByName(processName))
        {
            process.Kill();
        }
    }  
}
```

&nbsp; 

```csharp
// https://stackoverflow.com/a/35750677
public static class TimeExtensions
{
    public static TimeSpan StripMilliseconds(this TimeSpan time)
    {
        return new TimeSpan(time.Days, time.Hours, time.Minutes, time.Seconds);
    }
}
```

![Console App](console-app-running.png)

I’ve only tested it for a day and made some fixes to continue the app running.

You can tune in at [https://tunein.rpnradio.com/](https://tunein.rpnradio.com/) and we’ll debug any errors. 😀

## Future Plans

As of now, stations’ broadcast hours differ (sign on and sign off) and I want the app to be dynamic. Some plans for the app:

* Detect actual “dead air” time
* Detect actual “on air” time
* Custom playlist in VLC (not relying on “hook ups”)

Thanks for reading the blog post.
