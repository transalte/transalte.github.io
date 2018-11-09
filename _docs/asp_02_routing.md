---
title: Routing
layout: content
category: ASP
order: 2
permalink: /routing/
summary: selector~, matrix~, gate~, send / receive, send~ / receive~
---

# Routing

## Overview
The main method of routing messages and signals in Max is via the standard fixed point-to-point connections made with cords. However, there are a few options for making these connections more flexible and dynamic to allow for real-time user-controllable routing.

### Object: selector~


### Details
In the example for `selector~`, the arguments are number of outlets, and the initially open outlet; `selector~ 4 1` would provide a 4-in and 1-out routing option with the first inlet being the initially active signal being passed through.

To choose which signal passes through, an integer is sent to the `selector~` object; sending a 0 kills the signal, a 1 will let through the first input, 2 the second and so on.

### Common Mistakes

## Cordless Connections
### Object List
For messages:

* `send` and `receieve`
* or `s` and `r`

For signals (audio):

* `send~` and `receive~`


### Details
Cordless connections allow the developer to re-use the same connections repeatedly without running long cord-runs across a patch. For each `send` there can be multiple `receive` objects related to it. For example, in a stereo filter patch there can be one `send` attached to a `live.dial` that transmits a cut-off frequency value to the filter. Instead of connecting the dial directly to the left and the right filters, there can be a `receive` object connect instead i.e `send ---freq` and two `receive ---freq` object, each connected to the appropriate filter inputs.

### Three Hyphens
Common sense would suggest that if a dial is turned in a device, it should only have an affect in that device. However, in Max, cordless connections can travel between devices. For example, if a filter device was placed on every channel in Live, by turning the Cut Off dial, it would change the filter cut off on every channel. This has its uses, but to avoid this, the syntax for the `send` and `receive` objects, and their `~` counterparts must be changed. To restrict cordless connections to **within** there individual devices, place threes hyphens before the name such as `send ---cutOff` and `receive ---cutOff`. For signals, this would be `send~ ---cutOff` and `receive~ cutOff`.

The three hyphens, add a random number to the front of the name that will not be used in any other devices present in the current Live session thus avoiding any 'cross-talk' between effects.

> In the standalone verion of Max, the `---` should be replaced with `#0`.

### Common Mistakes
Spelling mistakes in the name of objects are a regular source of error. Check for typos. Including a space in the name will also cause problems.

Here is an example of the syntax that can be employed:

Good: `send ---filterFreq`

Bad: `send ---filter Freq`

Note that in the good example, instead of spaces in the name an uppercase letter has been used to make the name more readable.

Also, the issue of ring-fenced routing ie. the three hyphens in front of the name (again with no space in-between).

Also using the wrong type of send and receive may cause issues. Messages (solid colour cords) should use `send` or `s` and `receive` or `r`. Signals (hatched colour cords) should use `send~` and `receive~` - note there is no abbreviated version of the object for signals.

To check where a signal or message is being sent to, lock the patch and double-click the `send` object; the drop-down menu that appears will show a list of the `receive` objects related to that `send`. Selecting a `receive` from this menu will create a yellow highlight in the patch to show you where that particular `receive` is.


## Futher Reading / Research
