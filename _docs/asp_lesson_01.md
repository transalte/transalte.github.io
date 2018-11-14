---
title: L01 - First Device
layout: content
category: asp_lessons
order: 1
permalink: /firstdevice/
summary: Building a basic filter.
---

# Basic Filter


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


Translating Max into plain-speak, inlet 1 is the input for the filter and it expects either a signal (in this case it could be the audio from a synth or sample) or a **float** which is a fractional number.

> You may have noticed that some of the inlets will accept float values as well as audio signals. This adds more flexibility in the types of control you can use, i.e a static value from a dial (data) or an oscillator (audio signal).
> Another point to cover at this stage is the two types of numerical inputs in Max: Floats and Integers. Integers are full numbers i.e 1 or 47 and floats are values such as 20.5 or 100.25346. Floats will always have a decimal even if it is a full number such as 1. or 25. and even 0.

Inlet 2, controls the cut-off frequency for the filter and can be done so by an audio signal or a number (float again). Inlet 3 is for controlling resonance and has a range of 0. to 1.The range of the filter is in Hz as expected.

Looking at the outlets:

|I/O|DataFormat|Name/Description|
|:------|------|------|
|Outlet 1|signal|Low-pass Output|
|Outlet 2|signal|High-pass Output|
|Outlet 3|signal|Bandpass Output|
|Outlet 4|signal|Notch Output|

Due to the way this particular filter object is coded it is essentially four filters in one; each filter is constantly processing the incoming signal simultaneously and outputting a filtered signal. The user simply selects which filter they want to use by attaching a cord to that particular filter type's outlet.



## Connections and Controlling Values
The `svf~` object only has one inlet for audio input meaning that is effectively a mono processor. To quickly get a proof-of-concept up and running the device will be built as a mono processor initially.

/filter02.png

In the image above, the left and right outlets from `plugin~` are connected to the first inlet of a `*~` object before being sent into the `svf~` object. This multiplication object combines both left and right signals together and then multiplies their amplitude by 0.5 - this halves the amplitude of the signal avoiding any clipping when we hear the signal on output after processing.

> In Max, the amplitude of a signal is describes linearly i.e not in dB. When we multiply a signal by 1. (note the float) it's amplitude is unchanged; when multiplied by 0.5 it is halved. Multiply by 2. and the amplitude is doubled.

The first outlet of `svf~` (the Low-Pass outlet) is then connected to both the left and the right inlets of `plugout~`. The means that we are only processing the left channel of the incoming audio but are hearing it out of both the left and right channels after processing.

The next stage is to add controls for the filter’s frequency and resonance parameters. There are a number of objects that can be utilised as controls for objects but during the prototype stage the quickest ones to use are the number objects; the float (shortcut **f** on the keyboard) and the integer (shortcut **i**) objects.

### Frequency
By connecting either a float or integer number box to the second inlet of`svf~` you can control the cutoff frequency by entering the value into the box (when the patch is locked), or by clicking and holding on the number box and pushing the mouse/trackpad etc. up and down. **svf~** Keep in mind that although the **svf~** frequency runs from 20-20000Hz, the number box will permits figures above and below this being sent to the **svf~**.

### Resonance
For the Resonance control, we need to be more specific about the type of number box as it runs from 0. to 1. meaning that it expects a float so that we can enter 0.004 or 0.9745. Connect a float number box here. Be very careful to not go over 1. as the resonance may damage your hearing.

/filter03.png

Once you have played an audio clip through this device and can hear the filter working save it as B00XXXXXXXX_filter. Note that the file extension for a Max For Live device is ***.amxd**.
