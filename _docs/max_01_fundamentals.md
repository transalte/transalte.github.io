---
title: Max Fundamentals
layout: content
category: Max
order: 1
permalink: /fundamentals/
summary: A reference for the fundamentals of Max / Max For Live
lastupdate: 03-12-2018
---
# Max Overview
Max is classed as a [visual programming language](https://en.wikipedia.org/wiki/Visual_programming_language) for music and multimedia. It has been used in a number of areas for composition, performance and research purposes.

Programmmes made in Max, refered to as *patches* are constructed by connecting objects within a *patcher*. These objects, which perform a range of different functions, are seen as boxes within the user-interface connected via inlets (at the top of the box) and outlets (at the bottom of the box) by 'cords'.

There are two versions of the programme; '[Max](https://www.cycling74.com)' and '[Max For Live](https://www.ableton.com/en/live/max-for-live/)' (currently both at V8.0.2 as of 29-11-2018)

The difference between the versions as that 'Max' is a standalone product whearas 'Max For Live' runs *inside* Ableton Live and allows for the development of 'devices' that run similar to plugins within the Live environment.

---

## The Basics

### Patches
Patches are the files that are generated from Max; a patch could be a filter, an instrument, an arpegiator, a synth, a sampler and so on. There are two main formats to be aware of:

- *.maxpat
- *.amxd

If a patch is for use in the standalone version of Max, the format would be **.maxpat**. When developing for Max For Live, the file format is **.amxd**.

Note that even though the file-format for a device built for Ableton Live is **.amxd**, that file can contain other 'sub-patches' that would be in the **.maxpat** format[^1].

Max patchers (from the standalone version) can also be converted into standalone executable files that can be run without the main programme being installed.

### Data Formats
There are six basic data formats that are utilised in Max that are transmitted between the objects.

- Integers (full numbers)
- Floats (numbers with a decimal point)
- Symbols (text)
- Lists (made of multiple Integers, Floats and/or Symbols)
- Bangs (to trigger a function)
- Signals (audio)


### Objects
As stated, the core functionality of Max is based on 'objects', the connections between them and the data being transmitted.

Objects are boxes within the programme that perform functions. These can range from simple mathematical calculations to complex filtering or routing.

As with any programme language, the key to learning Max is becoming familiar with that language and how the objects interact with each other and can be utilised to reach an end-goal.

The majority of objects within Max have no graphical aspect. Unless the object's purpose is specifically for the user-inteface, it will contain the objects's name followed by 'arguments' and/or 'attributes'. UI (user-interface) based objects can display numbers, sliders, dials, buttons, drop-down-menus sliders, number boxes, dials, table editors, pull-down menus, buttons, and other means of the end-user to interact with the patch.

---

## Object Details

### Connecting Objects

#### Standard Connections
Click on the *inlet* or *outlet* of an object and drag the cord to the *inlet* or *outlet* of another object. Upon release, the connection will be made. If the connection is not made, this is an indication that the data being sent from one object to another is not valid for those particular objects.


### Attributes and Arguments
Attributes for an object are settings or parameters that dictate how an object is to function. An attribute would change or set a function of the object and an argument would be the value attached.

An example of this is the `metro` object configured as `metro @active 250`. This metro is configured to be automaticaly generate 'bang' upon load at a rate of one bang per 250ms. The '@' indicates the attribute and the agrument is the integer.

Another example of an arguments-only object is `selector~ 4 1`; in this instance the object will have 4 inlets with the first being active.

### Hot and Cold Inlets

[![Hot and Cold Inlets](/assets/img/max_inlets.png)*Hot and Cold Inlets*](/assets/img/max_inlets.png)

In the example above, changing the left number box causes the `+` calculation to be executed. However, changing the right number box results in no output from the `+` object. Why is this? Max has a concept of 'hot' and 'cold' inlets; only data being passed to a 'hot' inlet will trigger the function of an object. The inlets of the `+` object in this case show the left inlet as red (hot) and the right as blue (cold). This means that only when information is sent to the left inlet will the calculation occur.

To make both inlets 'hot', so that a change in any of the inlets will trigger the calulation, can be achieved by sending a bang to the left inlet. From Tutorial 6 in the Max documentation:

>"It is common practice, when a cold inlet needs to produce output, to apply a bang message (via a button or trigger object) into the left-most inlet to force output."

[![Hot and Cold Triggers](/assets/img/max_inlets_2.png)*Hot and Cold Triggers*](/assets/img/max_inlets_2.png)

The `t` object is the `trigger` object and is used to send multiple messages based on a single input. Note that the order of transmission is from **right-to-left**. In this exampe, the integer from the right number box (i) is sent to the right inlet of `~`, then a bang (b) is sent to the left inlet of `+` to retrigger the calculation. Effectively, `+` will carry out its function with the last value sent into the left inlet when it receives a bang.



### Help Files and Details

There are a number of ways to gain further insight into an objects properties and parameters. For each object, the **Inspector** can be opened which will detail all of the parameters, arguments and attributes associated with that object.

The inspector can be opened by clicking an object to highlight it and pressing **cmd + i** (Mac) or **something + i** (Windows).

The associated helpfile can be opened by **Alt+clicking** the object.

Additionaly, hovering over an object's middle-left area will show a yellow arrow. Pressing this will open up a contextual window that will show all the available details of messages that can be sent to the object as well as related arguments and attributes.

Nb. A list of shortcuts can be found [here](/shortcuts)

---

## Patch Cords

There are two types of cord in Max for connecting objects when building effects or instruments. Each are coloured differenctly according to the information being transmitted.

- Signals (Audio): Hatched colors
- Data (symbols, lists, numbers): solid

### Patch Organisation

Cord connections can be 'angled', or 'segmented' to make them easier to position and to avoid overlaps and making patches easier to follow. Simply click on a cord to highlight it and press **cmd+y**.

For more options, right-clicking on a cord will display the contextual menu where there are options for deleting, aligning, routing (segmenting) and removing segments. It is also possible to change the colour of the cords. Cords can also be disabled and re-enabled from this menu.

#### Selecting Multiple Cords

Option+click (Mac) or Alt+click (Win) and drag over a number of cords.


---

## Patcher Details

### Configuring Max For Live

To configure Max For Live, if required, refer to the [help file](https://www.ableton.com/en/manual/max-for-live/#25-1-setting-up-max-for-live) that comes with Live.

### Creating Devices

When developing in *Max For Live*, patchers are referred to as devices and come in three 'flavours':

- Audio Effects: Filters, delays, distortions, compression etc.
- Midi Effects: Arpegiators, sequencers, MIDI manipulation etc.
- Instruments: Synths, samplers, drum-machines etc.

To initiate any of these use the Live Browser and open drag the appropriate template onto a track in Live.

>If you are unfamiliar with Live, or need a refresh, read the Live Concepts document[^2]

Press the **Max Edit Button** button to open the device in the Max Editor window to begin patching.

![Edit Device](https://cdn-resources.ableton.com/80bA26cPQ1hEJDFjpUKntxfqdmG3ZykO/static/images/manual/en/MaxEditButton_opt.d23fd1779565.png)*Max Edit Button*

The patch will open with the default input and outputs for that particular device.

![Audio Effect Device](https://cdn-resources.ableton.com/80bA26cPQ1hEJDFjpUKntxfqdmG3ZykO/static/images/manual/en/MaxPatcher_opt.b62dba32d3e6.png)*Default Audio Effect Template*

---

## Default Values
One issue with patches that utilise a lot of Live UI elements (and even standard Max UI objects) is that when the patch or device is first loaded, all the dials, menus, number boxes, tabs etc. all resort to the default (usually lowest) value. This can be problematic in a device with a filter for example whereby the first filter type is a lowpass and the minimum cut-off frequency on a dial is 20Hz - when the user opens the patch there will most likely be no sound as it is being filtered out.

To solve this issue a default value can be defined for each UI object in the inspector. To enter a default value, or an initial value as Max refers to it:

1. Open the **Inspector** for the specific object and under the **Parameter** section look for the checkbox beside ‘**Initial Enable’** and click it.
2. The ‘**Initial**’ parameter is now available and a chosen value can be entered

On a `live.dial`, the value can be entered directly. However, on `live.menu` or `live.tab`, the *index* value of the menu item must be entered.

To avoid counting through the menu choices, it is easier to set the values on the UI elements themselves *then* enable the **Initial Value** checkbox, this will take the current value and add it to the Initial parameter.

When applying these values to a `live.x`, object it also allows the user to hit the backspace button to return to that value in the Live Session.

>Note that when a Live session is saved and reloaded, Live will replace the Initial Value with the last saved values

---





[^1]: An example would be a synth using `poly~` or an effect using `bpatcher`
[^2]: <https://www.ableton.com/en/manual/live-concepts/> "Live Concepts"
