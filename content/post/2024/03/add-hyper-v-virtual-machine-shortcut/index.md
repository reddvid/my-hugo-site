---
title: "How To Add Hyper-V Virtual Machine Shortcut on Windows 11 (and Stream Deck)"
description: "Launch Hyper-V Virtual Machines using a shortcut (and Stream Deck)"
date: 2024-03-25T17:56:45+08:00
featuredimage: "/images/og/vm-shortcut-og.png"
ogimage: "/images/og/vm-shortcut-og.png"
hidden: false
comments: true
draft: false
tags: ["Hyper-V", "Virtual Machines", "VM", "Windows 11", "Stream Deck"]
categories: ["How To", "Tech"]
---

### Update 3/26: Adding a guide for opening/connecting to VM using Stream Deck.

[**Jump to guide**](#using-stream-deck-to-open-vm-connection)

<hr>

## Shortcut for Windows 11

Ever want to launch a VM directly without opening the main Hyper-V app first?
Follow this steps to create a Virtual Machine connection shortcut.

1. In your selected directory, right-click on an empty space to open the context menu. Within **New**, select **Shortcut**.

{{< figure src="add-new-shortcut-context-menu.png" caption="Create New Shortcut" >}}

2. In the *Create Shortcut* window, enter the target settings shown below:

```
    vmconnect.exe <Hyper-V Server IP Address> "<Name of VM>"
```

{{< figure src="shortcut-target-settings.png" caption="Shortcut Target Settings" >}}

where <**Hyper-V Server IP Address**> is the host IP address of the Hyper-V server. And "<**Name of VM**>" is the actual VM name as seen in Hyper-V window.

3. Click **Next**, and give your shortcut a name:

{{< figure src="shortcut-name.png" caption="Giving the shortcut a name" >}}

Below is an example of a shortcut for my Pop!_OS VM in my local machine (Shortcut Properties), where ***Pop!*** is the name of my Virtual Machine.

{{< figure src="shortcut-properties.png" caption="Sample of my VM" >}}

4. Before opening the shortcut though, we need to make it to **Run as administrator**. Instead of right-clicking and manually running as admin each time, let's change the shortcut's **Advanced** properties:

{{< figure src="shortcut-advanced-properties.png" width="60%" >}}
{{< figure src="shortcut-to-run-as-admin.png" caption="Changing Advanced properties" width="60%" >}}

And that's it for your shortcut to run your VM. 

You can change the shortcut icon and/or you can move or copy the shortcut to show in your Start Menu (either one of the paths below). 

```
    C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs
    C:\ProgramData\Microsoft\Windows\Start Menu\Programs
```

<hr>

## Using Stream Deck to open VM Connection

The shortcut created above can be used as the **App/File** for the **Open** function of the bundled **System** plugin for StreamDeck. As seen below:

{{< figure src="open-shortcut-streamdeck.png" caption="VM Shortcut in Stream Deck" width="80%" >}}

### Using BarRaider's Advanced Launcher

This requires installing the above-mentioned plugin, found here: [Marketplace](https://marketplace.elgato.com/product/advanced-launcher-d9a289e4-9f61-4613-9f86-0069f5897125) or [Old Plugins Page](https://apps.elgato.com/plugins/com.barraider.advancedlauncher) (note: both are version 1.8 as of this writing).

Once installed, add an **Advanced Launcher** button into your Stream Deck and do the following:

1. Customize your button icon and give your button a Title. Or not.
2. In **Application** > **Choose file...***, you need to navigate to ```C:/Windows/System32/``` and choose ```vmconnect.exe```
3. In **Arguments**, type in <br> ```<Hyper-V Server IP Address> "<Name of VM>"```, <br> where ```<Hyper-V Server IP Address>``` is the host IP address of the Hyper-V server. And ```"<Name of VM>"``` is the actual VM name as seen in your Hyper-V server. <br><br> (Note: Stream Deck might remove the double-quotes in cases where the VM name has no spaces)
4. Check the **Run as Administrator** checkbox.

In my case, as seen below, I customized my button properties further:

- **Instances: Checked**
- **Max Instances: 1** (since launching it will force disconnect previous instances)
- **Bring to front: Checked** (Stream Deck will try to make the VM window to front - or flash the taskbar icon)

{{< figure src="advanced-launcher-vm-streamdeck.png" caption="VM launcher using Advanced Launcher" width="80%" >}}


That's it for adding a Stream Deck button to open your Virtual machine!