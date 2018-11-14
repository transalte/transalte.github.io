---
title: BASIC FILTER
layout: content
category: asp_lessons
order: 1
permalink: /firstdevice/
summary: Building a basic filter.
---

# Section Overview


This device will be a straightforward stereo filter with the followwing features:

* selectable filter types
* user controllable frequency and resonance parameters
* all parameters visible and available for automation in Ableton Live

## Pt1. Input Meters

1. Create a new Max For Live **Audio Effect** device
2. Place the mouse cursor over an area where there are no objects and press the **n** key. This will bring up a new object that is empty and is waiting for you to type the name of an object into it.
3. Press **L** and you will see Max attempt to auto-complete your entry; the object to locate is the `live.meter~` object. Once you hit enter, it should appear on the patch.
4. Move the meter to the right of the `plugin~` object by dragging it with the mouse.
5. Hold down the **alt** key on your keyboard, click and drag with the mouse to the right to create another copy.
6. Click and drag from the bottom left outlet of `plugin~` to the almost invisible inlet on the top of the left `live.meter~` object. Hover over this inlet for a description.
7. Do this same again but from the bottom right outlet of `plugin~` to the duplicated `live.meter~`.
8. If you play an audio clip or intrument in Live you should see the meters reacting to the audio.

![live.meter~](/assets/img/liveMeters.png)

## Pt2. The Filter

This next object to add is the object that performs the actual filtering of the audio signal: `svf~`.

1. Within the Max Editor, press **n** on your keyboard for a new object
2. Press **s** on your keyboard to start the autocomplete, `svf~` may show up immediately and you can press enter on the keyboard to add it to the patch. If it doesn't show up, you can scroll through the list of objects beginning with '**s**' with the arrow keys until you find it or type in the rest of the objects name to filter out the unwanted objects.


The `svf~` object has three inlets at the top and four outlets at the bottom; when the mouse pointer is placed over them a comment box will appear providing details of the inlet, or outlets, properties. In this case, hovering over the top left inlet displays the following:

```
svf~:(signal/float) Input
```

This format of the inlet/outlet properties is the same for all objects within Max following the same structure:

```
ObjectName:(DataFormat)(InputName/Description)
```

If we refer back to the inlets and outlets of `svf~`, it tells us the following:


|I/O|DataFormat|Name/Description|
|:------|------|------|
|Inlet 1|signal or float|Input|
|Inlet 2|signal or float|Cut Off Frequency(Hz)|
|Inlet 3|signal or float|Resonance (0-1)|


Looking at the outlets:

|I/O|DataFormat|Name/Description|
|:------|------|------|
|Outlet 1|signal|Low-pass Output|
|Outlet 2|signal|High-pass Output|
|Outlet 3|signal|Bandpass Output|
|Outlet 4|signal|Notch Output|

Due to the way this particular filter object is coded it is essentially four filters running simultaneously; each filter is constantly processing the incoming signal a filtered signal.

The `svf~` object only has one inlet for audio input meaning that is effectively a mono processor. To quickly get a proof-of-concept up and running the device will be built as a mono processor initially.

![Mono Filter](/assets/img/asp_filter02.png)*Mono svf~ effect*

In the image above, the left and right outlets from `plugin~` are connected to the first inlet of a `*~` object before being sent into the `svf~` object. This multiplication object combines both left and right signals together and then multiplies their amplitude by 0.5 - this halves the amplitude of the signal avoiding any clipping when we hear the signal on output after processing.

> In Max, the amplitude of a signal is described linearly i.e not in dB. When we multiply a signal by 1. (note the float) it's amplitude is unchanged; when multiplied by 0.5 it is halved. Multiply by 2. and the amplitude is doubled.

The first outlet of `svf~` (the Low-Pass outlet) is then connected to both the left and the right inlets of `plugout~` sending the same signal to both the left and right channels, thus mono.

## Controls
The next stage is to add controls for the filter’s frequency and resonance parameters. There are a number of objects that can be utilised as controls for objects but during the prototype stage the quickest ones to use are the number objects; the `flonum` (shortcut **f** on the keyboard) and the `integer` (shortcut **i**) objects.


### Frequency
By connecting either a float or integer number box to the second inlet of`svf~` you can control the cutoff frequency by entering the value into the box (when the patch is locked), or by clicking and holding on the number box and pushing the mouse/trackpad etc. up and down. Keep in the audible range (20Hz to 20kHz) to hear the effect of the filter.

### Resonance
For the Resonance control, use a `flonum` object as the resonance paramter runs from 0. to 1. meaning that it expects a float so that we can enter 0.004 or 0.9745. Be very careful to not go over 1. as the resonance may damage your hearing.

![Filter Controls](/assets/img/asp_filter03.png)*Filter Controls*

Once you have played an audio clip through this device and can hear the filter working save it. Note that the file extension for a Max For Live device is `*.amxd` when using **Max For Live** and not `*.maxpat` which would be for **standalone** Max.

## Live UI Elements

From an end-user point of view these number boxes aren’t the most ergonomic, or familiar for Live users. A more recognisable object to use instead is `live.dial`.

When added to a patch the default range of the dial will be 0 to 127. This can be changed to suit whatever parameter is to be controlled via the **Inpector**.

There are also two outlets on `live.dial`; the left outlet transmits the range shown on the dial. The right outlet sends out values from 0. and 1. regardless of the range of the dial.

Connect a live.dial to svf~ as follows:

![live.dial connections](/assets/img/asp_livedial01.png)*live.dial connections*

The left dial by default will send values from 0 to 127 to the svf~, this is fairly useless for a filter so we will fix this by changing some values the inspector.

1. Open the **Inspector** for the `live.dial`.
2. Under the **Parameters** subsection change the **Type** from **Int (0-255)** to **Float**. When set as **Int**, we are capped at values between 0-255, with **Float** this limit is removed.
3. Change the **Range/Enum** to 20. 20000. (remember the period after the numbers and the space between those two values)
4. Change the **Unit Style** to **Hertz**.
5. Change the **Short Name** to **Freq** - this changes the UI text
6. Change the **Long Name** to **“Filter Frequency”** - this is the text allocated to the envelope names in Ableton Live and as the name contains a space you must include the quotation marks


The dial should now run from 20 to 20kHz albeit very hard to fine-tune the more useful lower values. To skew the distribution of the numbers across the dial, open the the **Inspector** again and set **Exponent** to 3.3333 - this will weight the values at the lower end of the dial for more precision.

For the second `live.dial`, for Resonance control, change the **Short** and **Long Names** to **Resonance**. Connect the right outlet (0. to 1. range) to the appropriate inlet on the `svf~` object.


## Stereo Implementation

Making this patch stereo is a case of duplicating the existing filter section or creating another `svf~` object and connecting the **Freq** and **Res** controls to both filters in the appropriate inlets. Note that the patch now utilises note the **L** and **R** channels coming from `plugin~`.

There could be separate dials for left and right but at this point it is best to avoid overcooking such a basic device.

![Stereo Filter](/assets/img/asp_filter04.png)*Stereo Filter*

## Cord Organisation
Notice that in the example screenshot the cords look a bit tidier than in your patch. In the screenshot, the cords bend around the objects which make it tidier and easier to follow. To do this, click a cord (it will go blueish) and press **cmd+y.** You can then click and grab straight segments of the cord and move it about. The colour of the cords has also been changed, this makes it much easier to follow the signal paths. This is done by selecting a cord by clicking on it (shift and click others to add to the selection), right clicking on the cord and selecting Color from the context menu.

Keeping your patches organised like this is a good habit to adopt as early as possible for ease of following signal flow and finding errors. Also, when you eventually hit a brick wall with a patch, tidying it up can lead to a moment of zen while being distracted with the drudgery of aligning objects and cords…

## Filter Type Selection
As stated earlier, the `svf~` object has four outlets. The object that will allow the user to switch between these four signals is `selector~`.

![selector~ object](/assets/img/asp_selector01.png)*selector~ object*

From the screenshot above, the `selector~` object has two numbers after it’s name. These are *arguments* which configure the initial states of an object that may or may not be overridden by the user.

In this case, the first argument dictates how many inlets the object will have (4 in this case), and the second argument dictates which of these inlets will be routed to the outlet when the patch initialises (loads for the first time).

 When object's name is selected after calling up a new blank object (**n** on the keyboard) a dropdown menu is displayed when the spacebar is pressed as shown in the screenshot below. This menu will diplay a list of all the messages and arguments that are related to that object.

 ![selector~ object](/assets/img/asp_selector02.png)*selector~ messages and arguments*

 When `selector~` receives an integer in the first inlet, it routes the audio from the selected inlet to the outlet. Receiving **1** lets through the first signal inlet, **2** the second and so on. When it receives a **0** no signal is let through, effectively acting as a gate.

Once the arguments are in place, add the `selector~` objects into the patch as follows:

![selector~ object](/assets/img/asp_selector03.png)*selector~  configuration*

## Using Menus

Using numbers for the filter selection will be fairly meaningless to the end-user so a better method is to use menus related to the parameters.

Add a `live.menu` object to the patch and notice that the drop-down menu is already populated with the text ‘one’, ‘two’ and ‘three’.

![live.menu object](/assets/img/asp_livemenu01.png)*live.menu object*

If you hover over the outlets, the first outlet provides the ‘Item Index’ of the menu selection (0, 1, 2, 3 and so on) for each entry. The second outlet provides the ‘Selection Symbol’ ie. the actual text of the item chosen.

|Item Number|Item Index|Item Symbol|
|:------|------|------|
|First|0|One|
|Second|1|Two|
|Three|2|Three|

To populate the `live.menu` with the relevent filter types or `svf~`:

1. Select the `live.menu` and open up the **Inspector**
2. Look under **Parameter** section for **Range/Enum** and replace the **one two three** text with the type of filters coming into the inlets of selector: `Lowpass Highpass Bandpass Notch`

> If the **Parameter** section in **Inspector** is not visible, click **All** in the top section of the **Inpector** panel.

Making these changes will produce the following outcomes.


| Item Number | Item index | Item symbol |
| ----------- | ---------- | ----------- |
| First       | 0          | Lowpass     |
| Second      | 1          | Highpass    |
| Third       | 2          | Bandpass    |
| Fourth      | 3          | Notch       |

The issue here is that when the user selects **Lowpass** from the menu, it will output a **0** to the `selector~` and shut down the signal completely.

To get around this misalignment of values a `+` object between the `live.menu` and the `selector~` object with the argument `1`.

![live.menu and selector](/assets/img/asp_menuSelector.png)*live.menu & selector~*


Now, an output of 0 from the `live.menu` triggers a 1 on `selector~` which lets the selected filter outputs through correctly.

## Presentation Mode

At this stage, the patch is only controllable inside the **Max Editor**. Back in **Live**, the interface for the device most-likely will show the top section of the patch instead of a porperly designed user-interface (UI).

![presentation mode](/assets/img/asp_pres01.png)

Open up the device again and go into **Presentation Mode** by either clicking **drawing-board** icon at the bottom left of the patch’s window, going to **View > Presentation** in the main menu or pressing **alt+cmd+e.** See the screenshot below:

![presentation mode](/assets/img/asp_pres02.png)

You will see a blank patch. This is because we need to add our UI controls to this view of the patch.

To make objects visible in the UI:

1. Come out of **Presentation Mode** (click the icon or shortcut again)
2. Click on the object you want to see in the UI to select them and then right-click to bring up the context menu and select **Add to Presentation**. The object should have a red outline on it now indicating it’s now active in the **Presentation Mode**
3. Do this for all the relevant UI controls objects. The keyboard shortcut for this is **cmd+shift+p** after selecting the object

Go back to **Presentation Mode** to see the object(s) with no patch cords visible. it is possible to reposition objects anywhere in the screen and, depending on the object, resize it with the handles at the bottom right that appear when hovering the mouse over that area.

The very final step is to make the UI visible to Live:

1. Right-click on an empty area (make sure you have not clicked on any objects) of the patcher when in Presentation Mode and select Patcher Inspector from the context menu that pops up.
2. Under the View heading in the Inspector, check the box to the right of ‘Open in Presentation’ and save your patch.

Back in Live, the devices UI should now contain all of the objects you added to the Presentation.

As a final addition, add a **live.gain~** object to the patch that sits between the output from the **selector~** objects and the **plugout~** as shown below; make sure that you change the **Short Name** and **Long Name** to something appropriate for the control in the **Inspector**, then add this to the UI via **Presentation Mode**

![presentation mode](/assets/img/asp_pres03.png)

In the screenshot below, some `comment` objects (press **c** on the keyboard to bring this up) have been added to act a labels for some of the objects.

![presentation mode](/assets/img/asp_pres04.png)
