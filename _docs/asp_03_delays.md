---
title: Delays
layout: content
category: ASP
order: 4
permalink: /delays/
summary: tapin~ / tapout~, delay~
---

# Delay Effects

While there are a number of different types of delays, they are all based around the same concept; a signal comes in and passes clean through to the output while it is also sent through a delay block to be joined with the dry signal.

What happens in the delay circuit is where the differences lie. Standard controls found on these effects are **delay time**, **feedback**, **mix/blend**, and depending on the signal flow (topology) the output can vary wildly between one design and another.

For example, if a filter is placed in the feedback loop, the output can be similar to the sound of a tape delay by changing the frequency content as each echo passes though the feedback loop again. That same filter could also be placed before or after the feedback loop, again giving a completely different sound. Alternatively, other processes can be added into the feedback loop for even more options.

The device being made in this section will be a basic delay, starting as a simple mono delay and ending up as a ping-pong filter delay with user controllable frequencies, filter types, delay times etc.

## Objects
There are two objects that can be used to delay[^1] audio; [delay~] and a more useful pair of objects [tapin~] and [tapout~].

The main difference between the object(s) is that the audio output from [delay~] cannot be fed back into itself to create a feedback loop. Another feature of [delay~] is that the delay time can be set in samples as opposed to milliseconds with the [tapin~] and [tapout~] combo.


> At the standard rate of 44.1 kHz, there are 44,100 values describing the signal per second so these delays can verge towards being more of a phaser type effect at shorter delay values.

With [tapin~] and [tapout~], it is possible to setup a feedback loop for more interesting effects therefore these are the objects we will use for this exercise.

[^1]: delay footnote
