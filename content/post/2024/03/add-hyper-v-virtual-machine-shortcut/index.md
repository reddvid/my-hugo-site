---
title: "How To Add Hyper-V Virtual Machine Shortcut on Windows 11"
description: "Launch Hyper-V Virtual Machines using a shortcut"
date: 2024-03-25T17:56:45+08:00
featuredimage: "/images/og/vm-shortcut-og.png"
ogimage: "/images/og/vm-shortcut-og.png"
hidden: false
comments: true
draft: false
tags: ["Hyper-V", "Virtual Machines", "VM", "Windows 11"]
categories: ["How-To", "Tech"]
---

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

