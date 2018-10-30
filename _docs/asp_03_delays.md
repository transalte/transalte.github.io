---
title: Delays
layout: asp
category: ASP
order: 4
---

# Delay Overview

While there are a number of different types of delays, they are all based around the same concept; a signal comes in and passes through to the output while it is also sent through a delay block to be joined with the dry signal. What happens in the delay circuit is where the differences lie. Standard controls found on these effects are **delay time**, **feedback**, **mix/blend**, and depending on the signal flow (topology) the output can vary wildly between one design and another.

For example, if a filter is placed in the feedback loop, the output can be similar to the sound of a tape delay by changing the frequency content as each echo passes though the feedback loop again. That same filter could also be placed before or after the feedback loop, again giving a completely different sound. Alternatively, other processes can be added into the feedback loop for even more options.

The device being made in this section will be a basic delay, starting as a simple mono delay and ending up as a ping-pong filter delay with user controllable frequencies, filter types, delay times etc.

+++

## Stage 01: Delay Objects
There are two objects that can be used to delay audio; **delay~** and a more useful pair of objects **tapin~** and **tapout~**.

The main difference between the object(s) is that the audio output from **delay~** cannot be fed back into itself to create a feedback loop. Another feature of **delay~** is that the delay time can be set in samples as opposed to milliseconds with the **tapin~** and **tapout~** combo.

	At the standard rate of 44.1 kHz, there are 44,100 values describing the signal per second so these delays can verge towards being more of a phaser type effect at shorter delay values.

With **tapin~** and **tapout~**, it is possible to setup a feedback loop for more interesting effects therefore these are the objects we will use for this exercise.

Create a new Max Audio Effect and open it in the Max Editor. Recreate the patch below:

/d01.png

There are two ‘streams’ of audio in this patch so far; the (original) dry signal passing directly from **plugin**~ to **plugout**~ and the same signals from **plugin**~ feeding into the **tapin~ / tapout~** delay section via a ***~ 0.5**.

The ***~ 0.5** object is there to sum the left and right inputs together and halve the amplitude; this effect is a mono delay at the moment so this is fine. Both streams then are combined, or summed together, at the **plugout**~ object.

There are arguments in both the **tapin~** and **tapout~** objects. To understand these, consider that with digital hardware delays, such as guitar pedals, there is a maximum delay time limit that is usually stated in seconds. This is because the incoming signal has to be stored temporarily in a buffer before it is sent out. The **tapin~** object is the same and needs to be provided with a value dictating how long the maximum delay time can be. In the example above, the argument is **10000** providing a maximum delay time of **10 seconds**. The **tapout~** object is where we control the actual delay time we want to use also in milliseconds. Therefore, in this example the delayed signal occurs **250ms** after the dry signal.

+++

## Stage 02: Time, Feedback and Mix Controls

### Time
The first of the controls to be added to this delay is going to be for controlling the delay time. There are a number of ways to carry this out, all of which require a direct connection to the **tapin~** object to override the argument and for the delay time to be in **milliseconds**.

An integer number box ('**i**' on the keyboard for this shortcut) can be connected to **tapin~** and the user can enter the delay time in ms manually. This can be replaced later by a **live.dial** and so on for the UI. However, this can be less than ideal when working in a DAW as the user would have to either dial in the delay time by ear to get sync with the tempo of the song, or have to calculate the delay time manually. A better method would be using tempo-related time-divisions...
