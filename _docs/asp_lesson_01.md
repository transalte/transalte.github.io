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

## 1. Input Meters

1. Create a new Max For Live **Audio Effect** device
2. Place the mouse cursor over an area where there are no objects and press the n key. This will bring up a new object that is empty and is waiting for you to type the name of an object into it.
3. Press L and you will see Max attempt to auto-complete your entry; the object to locate is the live.meter~ object. Once you hit enter, it should appear on the patch.
4. Move the meter to the right of the plugin~ object by dragging it with the mouse.
5. Hold down the alt key on your keyboard, click and drag with the mouse to the right to create another copy.
6. Click and drag from the bottom left outlet of plugin~ to the almost invisible inlet on the top of the left live.meter~ object. Hover over this inlet for a description.
7. Do this same again but from the bottom right outlet of plugin~ to the duplicated live.meter~.
8. If you play the clip in Live you should see the meters reacting to the audio.

![live.meter~ object(s)](/assets/img/live.meters.png)
