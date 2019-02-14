---
title: Polyphonic Synth
layout: content
category: aasp_lessons
author: Robert Goldie
order: 1
permalink: /polysynth/
summary: Creating a polyphonic synth
lastupdate: 14/02/2019
---

# POLYPHONY OVERVIEW
Polyphony in synths refers to the ability to play more than one note at a time i.e chords. The previous monophonic synth only permitted a single note to played [^poly].

Polyphony is achieved in Max by having a top level patch which, in addition to the user-interface elements, MIDI and audio outputs, also contains a `poly~` object which holds the 'guts' of a single-voice (monophonic) synth. The `poly~` object effectively provides an instance of this 'voice' for each key pressed or note received.

---

## Patch Structure

In **Max For Live**, it is best to work within a folder layout out as follows:

* **MySynth** (folder)
	* **MySynth.amxd** (main patch)
	* **MySynthCore.maxpat** (patch that contains the guts of the synth)

The `poly~` object, instead of containing the actual synth content like an encapsulation, *references* a separate ***.maxpat** file.

The syntax for `poly~` can explain how it works:

`poly~ MySynthCore.maxpat 8 @steal 1 @target 0`

In this example, external patch 'MySynthCore' is being referred to and the synth would have a maximum of 8 voices, or instances allowing the user to play 8 notes and hear them. If more notes are played, depending on the **steal** argument, priority can be given to the first or last notes in terms of what notes to kill to allow the new notes to be heard...

### Poly Arguments
There are a number of arguments and attributes for the `poly~` object; listed below are the most relevent for this patch. More details can be found in the `poly~` reference file [here](https://docs.cycling74.com/max7/maxobject/poly~).

|Argument|Type|Example|Function|
|-|-|-|
|patcher-name|symbol|MySynthCore.maxpat|name of patcher to be loaded|
|@voices|integer|16|Corresponds to the number of available voices|
|@steal|integer|@steal 1|will send incoming data to currently busy notes cutting them off|
|@target|integer|@target 0|sends input to all voices|

Referring to the previous example

`poly~ MySynthCore.maxpat @voices 8 @steal 1 @target 0`

The patch will allow for 8 voices, note stealing is enabled and any parameter changes/information is being sent to each of those 8 voices i.e if the user changes an oscillator tuning or a filter frequency, that change will be reflected on each voice.

---



## Implementation

The screenshot(s) below show the top level **.amxd** and the core **.maxpat** files of a basic polyphonic synth. Note that in the example, wireless send and receives are used for transmitting the amp envelope and filter parameters:

[![Polyphonic Patch](/assets/img/aasp_poly_01.png)*Polyphonic Patch*](/assets/img/aasp_poly_01.png)

[![Polyphonic Voice](/assets/img/aasp_poly_01b.png)*Polyphonic Voice*](/assets/img/aasp_poly_01b.png)


### MySynth Poly.amxd
On the top level ***.amxd** patch, in addition to the new `poly~` object, there is a `prepend` object with the argument **midinote**. The 'midinote' symbol (text) is placed in front of each 'note-on/off velocity' message from `midiparse` letting the `poly~` object know that the incoming data in that particular inlet is note information.

For the envelope and filter controls, all the parameters are joined together in a 'list'message i.e 'attack decay sustain release' (1. 125. 0. 150. for example) with the `pan` object. These lists are then transmitted into the `poly~` object arriving at `r ---AmpEnv` and `r ---FltControls` . The messages are then split up via the `unpack 0. 0. 0. 0.` object and connected to the relevant inlets.

>Note that as the values being packed together are floats, the '**.**' must be included with the placeholder arguments. Likewise, at the unpacking stage, the '**.**' must also be included in a `unpack` object to avoid converting a float to an integer.

There are two objects for creating lists that are similarly named; the aforementioned `pak`, and `pack`. The difference between the objects is when the list is sent; items collated together with `pack` will only get sent as a list when data is received in it’s *first* inlet. With `pak`, the entire list is transmitted when data is received in *any* inlet (including any placeholder arguments).

### MySynthCore.maxpat
To re-open the referenced patch from the top level, double-click the `poly~` object while the patch is locked (similar to sub patches).

The main difference between this external patch and a standalone mono synth is that the input and output objects are slightly different. In a standard encapsulated subpath the ins are outs are `inlet` and `outlet` and the numbering happens automatically. In `poly` this changed to `in`, `out` (for message information) and `in~` and `out~` with a number manually entered afterwards i.e `out~ 1` and `in 1` in this example.

The main addition of importance here is the `thispoly~` object which handles the status of voices being active/busy (in use from a note press) or not. This object is used to switch voices on and off. When used in tandem with the **@target 0** argument in `poly~`, every voice in the patch will receive the same parameter changes. The `thispoly~` voice on/off function can be controlled by using the the 1st and 3rd outlets of `adsr~`; the 3rd outlet will turn the voice on an off while when an audio signal enters `thispoly~` the voice is activated, when the gain of that signal reaches 0 , the voice is deactivated. Both are connected in this instance as a fail safe but it is possible to use either or.

## Synth Structure

Now the structure of the synth is as follows:

MIDI In > `poly~` for voices > Audio Effects > `plugout~`

Note that depending on the features included, the audio signal path may change to stereo either inside `poly~` or at the Audio Effects stages.


[^poly]:Although chords can be played by tuning oscillators in various combinations of semitones, this is not polyphony.
