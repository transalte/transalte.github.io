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

----

## Stereo Implementation

Making this patch stereo is a case of duplicating the existing filter section or creating another `svf~` object and connecting the **Freq** and **Res** controls to both filters in the appropriate inlets. Note that the patch now utilises note the **L** and **R** channels coming from `plugin~`.

There could be separate dials for left and right but at this point it is best to avoid overcooking such a basic device.

![Stereo Filter](/assets/img/filter04.png)*Stereo Filter*

## Cord Organisation
Notice that in the example screenshot the cords look a bit tidier than in your patch. In the screenshot, the cords bend around the objects which make it tidier and easier to follow. To do this, click a cord (it will go blueish) and press **cmd+y.** You can then click and grab straight segments of the cord and move it about. The colour of the cords has also been changed, this makes it much easier to follow the signal paths. This is done by selecting a cord by clicking on it (shift and click others to add to the selection), right clicking on the cord and selecting Color from the context menu. 

Keeping your patches organised like this is a good habit to adopt as early as possible for ease of following signal flow and finding errors. Also, when you eventually hit a brick wall with a patch, tidying it up can lead to a moment of zen while being distracted with the drudgery of aligning objects and cords…
