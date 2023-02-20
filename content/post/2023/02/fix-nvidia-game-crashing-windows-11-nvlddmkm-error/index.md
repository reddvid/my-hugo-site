---
title: "Fix NVIDIA Game Crashing & Black Screen on Windows 11 (nvlddmkm Error)"
description: 
date: 2023-02-10T04:30:05+08:00
featuredimage: "/images/og/nvidia-error.png"
ogimage: "/images/og/nvidia-error.png"
hidden: false
comments: true
draft: false
tags: ["NVIDIA", "How to fix", "Fix nvlddmkm error", "Game crash", "Black screen"]
categories: ["Stories","Gaming"]
---
[Skip the bullsh*t, just show me your solution! (please)](#solution-for-me-at-least)

## Some Background First

I couldn’t play (mid-tier) games long enough without them crashing after 30-minutes, or worse is just about 10-minutes - which happens when I am just casually playing Dota 2 AI scripts to waste some time. I thought there was just a problem (again) with my RAM sticks but two memory checker programs reported no errors on them. Then I just went and accepted the crashes so I could use my time doing more important things.

But the crashes happened with other games like High on Life, Control, and CODMW. And after trying to watch some Dota 2 match in-game, the crash happened - and I am fed up.

(But thankfully, I am not getting any bug checks or BSODs)

## The Problem

I checked Windows Event Viewer for any logs and there it is!

{{< figure caption="nvlddmkm Error in Windows Event Viewer" src="nvlddmkm-error-event.png" >}}

{{< figure caption="Error Details" src="event-error-details.png" >}}

```xml
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
	<System>
		  <Provider Name="nvlddmkm" /> 
		  <EventID Qualifiers="0">0</EventID> 
		  <Version>0</Version> 
		  <Level>2</Level> 
		  <Task>0</Task> 
		  <Opcode>0</Opcode> 
		  <Keywords>0x80000000000000</Keywords> 
		  <TimeCreated SystemTime="2023-02-09T15:15:31.3464479Z" /> 
		  <EventRecordID>138086</EventRecordID> 
		  <Correlation /> 
		  <Execution ProcessID="4" ThreadID="4588" /> 
		  <Channel>System</Channel> 
		  <Computer>David-PC</Computer> 
		  <Security /> 
	  </System>
	<EventData>
	  <Data>\Device\Video3</Data> 
	  <Data>Error occurred on GPUID: 2b00</Data> 
	  <Binary>00000000020030000000000000000000000000000000000000000000000000000000000000000000</Binary> 
  </EventData>
</Event>
```

## Solutions from the Web

There are a BUUUNNCH of users experiencing this issue, and some solutions vary from re-installing GPU drivers, cleaning the GPU from dust, putting heat sink to RAM sticks, changing the PSU, replacing the GPU, and even not mixing AMD CPU and NVIDIA GPU on the same system.

As of my situation, I can’t do most of the above advised solutions, though I tried re-installing my graphics driver using [DDU](https://www.guru3d.com/files-details/display-driver-uninstaller-download.html).

But there was one reply among them which they tried rolling back their driver and fixed their issue.

## Solution (for me, at least)

I rolled back/re-installed my graphics drivers to the last known working one - which is NVIDIA’s December 8 Game Ready Driver 527.56 update.

You can try to roll back yours by downloading your last working driver [here](https://www.nvidia.com/Download/Find.aspx?lang=en-us).

This was the driver where I played Assassin’s Creed Odyssey without a hiccup and I’ve tested it by watching an hour-long Dota 2 game.

I will update this post after testing with the other games mentioned above.

<small>Maybe this was a move to force me to replace my weak RTX 2060 huh</small>
