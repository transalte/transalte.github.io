---
title: User Interface
layout: content
category: Max
order: 1
permalink: /gui/
summary: User Interface, Layout, GUI Techniques
lastupdate: 14/03/2019
---

## GUI Design and Implementation
The GUI(graphical user interface) of a device / patch in Max is referred to as the **Presentation Mode** of a patch and consists of what the end-user will see and interact with.

When developing in **Max For Live**, this is the front-end of the device that is seen in the host (Live) at the bottom of the screen for the tracks. When developing for **Live**, there are limitiations on the vertical height (169 pixels) of a devices UI for visisbility; horizontally there are less restrictions but it is best to avoid the user from scrolling left and right to see the full UI of a device. When working in Max-standalone, the UI can be as tall and long as required.

There are a number of methods to make better use of the space available for **M4L** devices; tabs, expanding and retracting a device horizontally or having the device pop-out in it's own window are possible.

### GUI Options
There are a number of methods to make better use of the space available for **M4L** devices; tabs, expanding and retracting a device horizontally or having the device pop-out in it's own window are possible.

#### Default M4L

The normal way to create a UI is to work within the vertical limit of the **Live** effects pane. This approach is absolutely fine for those devices that can fit all their controls into reasonably compact, single layer UI.

The example below shows that with careful planning, a complex device can fit into a reasonably small area.

[![Northen Lights Delay](/assets/img/nl.png)*Northern Lights Delay*](/assets/img/nl.png)

For details of the fundamentals of **Presentation Mode**, refer to the [layout](/basicfilter/#presentation-mode) section of the Basic Filter content.

For busier, more complex devices that require more screen-space consider the following options.

#### X-Axis Expand

This UI trick allows the designer to 'hide' certain elements of a device's UI 'off-screen' until the user presses a button. This can be implemented to show or hide controls for effects, modulation etc. or any elements that do not need to visible at all times.

[![UI Expand](/assets/img/ui_expand_01.png)*UI Expand Patch*](/assets/img/ui_expand_01.png)

##### How It Works
The user presses the `live.toggle` (generating a 0 or 1), that number passes through `sel 0`; a zero will bang the `t 100`, a 1 will trigger the `t 300`. These numbers then pass through the `messagebox` to create the message `set width 100` or `setwidth 300`. This message passes to `live.thisdevice` which adjusts the width.

##### Presentation Mode
This is how the Patch is layed out in the **Max Editor**. Note that the `live.dial` objects are more than 100 pixels out from the left of the device.

[![UI Expand Presentation](/assets/img/ui_expand_02.png)*UI Expand Presentation*](/assets/img/ui_expand_02.png)

##### Device in Live
The next two screenshots show the different views of the device when the toggle is used:

[![UI Expand Demo Normal](/assets/img/ui_expand_03.png)*UI Expand Demo Normal*](/assets/img/ui_expand_03.png)

[![UI Expand Demo Expanded](/assets/img/ui_expand_04.png)*UI Expand Demo Expanded*](/assets/img/ui_expand_04.png)

#### Pop-out Window
This method places the device into it's own pop-out window similar to how Audio Units or VSTs look when opened.

##### How It Works
Similar to the expansion example above, the user presses a button to open up a window that displays the device. The screenshot below shows how this is implemented.

[![UI Pop-out Window Patch](/assets/img/ui_popout_01.png)*UI Pop-out Window Patch*](/assets/img/ui_popout_01.png)

Effectively, the entire device is [encapsulated](/encapsulation/) into its own sub-patch[^pop1]. There are two elements to this functionality; the button in the top level used for launching the window, and the receiving `thispatcher` object in the sub-patch.

When these are configured as in the example, it is important that the **Inspector** for both the top level *and* the sub-patch is configured so that the box for  **Open In Presentation** is checked.

> At the top-level, this will ensure that the button/toggle etc. is visible to the user. In the sub-patch, this means that the window will open up in Presenation Mode. If the subpatch is not configured in tehi way, the user will simply see the Max Editor view with cords and object etc.

Once the **Inspector** is configured, make sure that the subpatch is in **Presentation Mode** (click the whiteboard icon at the bottom), and resize the window to fit the UI accordingly[^pres]:

[![UI Pop-out Window UI](/assets/img/ui_popout_02.png)*UI Pop-out Window UI*](/assets/img/ui_popout_02.png)

This resized sub-patch will be the size of the window that opens up containing the patch.

[^pop1]: In this example, `plugin~` and `plugout~` are outside of the patch, but they could also be in the subpatch.

[^pres]: Refer to the full help files on `thispatcher` for other methods included specifiying where the window will popo-up
