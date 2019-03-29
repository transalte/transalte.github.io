---
title: Delay Effects
layout: content
category: asp_lessons
order: 2
permalink: /delays/
summary: Creating delays
lastupdate: 03-12-2018
---

## Delay Effects

While there are a number of different types of delays, they are all based around the same concept; a signal comes in and passes through to the output while it is also sent through a delay block to be joined with the dry signal. What happens in the delay circuit is where the differences lie. Standard controls found on these effects are **delay time**, **feedback**, **mix/blend**, and depending on the signal flow (topology) the output can vary wildly between one design and another.

![Delay Diagram](/assets/img/delay_diagram1.png)*Basic Delay Diagram*

For example, if a filter is placed in the feedback loop, the output can be similar to the sound of a tape delay by changing the frequency content as each echo passes though the feedback loop again. That same filter could also be placed before or after the feedback loop, again giving a completely different sound. Alternatively, other processes can be added into the feedback loop for even more options.

The device being made in this section will be a basic delay, starting as a simple mono delay and ending up as a ping-pong filter delay with user controllable frequencies, filter types, delay times etc.

### Delay Objects
There are two objects that can be used to delay audio; **delay~** and a more useful pair of objects **tapin~** and **tapout~**.

The main difference between the object(s) is that the audio output from **delay~** cannot be fed back into itself to create a feedback loop. Another feature of **delay~** is that the delay time can be set in samples as opposed to milliseconds with the **tapin~** and **tapout~** combo.

>At the standard rate of 44.1 kHz, there are 44,100 values describing the signal per second so these delays can verge towards being more of a phaser type effect at shorter delay values.

With **tapin~** and **tapout~**, it is possible to setup a feedback loop for more interesting effects therefore these are the objects we will use for this exercise.

Create a new Max Audio Effect and open it in the Max Editor. Recreate the patch below:

![basic delay](/assets/img/d01.png)

There are two ‘streams’ of audio in this patch so far; the (original) dry signal passing directly from **plugin**~ to **plugout**~ and the same signals from **plugin**~ feeding into the **tapin~ / tapout~** delay section via a ***~ 0.5**.

The ***~ 0.5** object is there to sum the left and right inputs together and halve the amplitude; this effect is a mono delay at the moment so this is fine. Both streams then are combined, or summed together, at the **plugout**~ object.

There are arguments in both the **tapin~** and **tapout~** objects. To understand these, consider that with digital hardware delays, such as guitar pedals, there is a maximum delay time limit that is usually stated in seconds. This is because the incoming signal has to be stored temporarily in a buffer before it is sent out. The **tapin~** object is the same and needs to be provided with a value dictating how long the maximum delay time can be. In the example above, the argument is **10000** providing a maximum delay time of **10 seconds**. The **tapout~** object is where we control the actual delay time we want to use also in milliseconds. Therefore, in this example the delayed signal occurs **250ms** after the dry signal.

### Delay Times and Controls

#### Time
The first of the controls to be added to this delay is going to be for controlling the delay time. There are a number of ways to carry this out, all of which require a direct connection to the **tapin~** object to override the argument and for the delay time to be in **milliseconds**.

An integer number box ('**i**' on the keyboard for this shortcut) can be connected to **tapin~** and the user can enter the delay time in ms manually. This can be replaced later by a **live.dial** and so on for the UI. However, this can be less than ideal when working in a DAW as the user would have to either dial in the delay time by ear to get sync with the tempo of the song, or have to calculate the delay time manually. A better method would be using tempo-related time-divisions...

#### Max Time Formats
Max Time Formats are how Max understands and processes timing information and there are two main headings: **Fixed Time Values** and **Tempo-Relative Time Values**.


> **Fixed Time Values**:  Milliseconds; Hours/Minutes/Seconds; Samples; Frequency (Hz)
>
> **Tempo-Relative Time Values**: Bars/Beats/Units (BBU); Ticks; Note Values.



**Fixed** values are those which stay the same regardless of the tempo of the track; 10 milliseconds will always last 10 milliseconds if the tempo is 147bpm or 98bpm. For example; a delay of 125ms may be perfectly in time with the tempo of the track at one bpm but at another bpm it could be out by a large margin.

**Tempo-Relative** values are those that which will change depending on the tempo in order to keep in sync; 1 bar at 3bpm is obviously longer than 1 bar at 300bpm. In the **Bars/Beats/Units** format (BBU), the largest division of time is a **Bar** or multiple of. A division down, and we are into **Beats**, a division down from beats are **Units** which are referred to as ticks. **Ticks** are the smallest time divisions of a beat and there are **480** of them per beat, half a beat being 240 ticks and so on.

In addition to **BBU** Max also uses **Note Values** which are shorthand for a range number of time divisions. These are listed below and are an easy way to get access to tempo relative time divisions. A single ‘whole note’ in this time format is a bar.

| Note Value | Text  | Ticks |
|:--|:--|:--|
1nd | Dotted whole note | 2880 |
1n | Whole note | 1920 |
1nt |Whole note triplet|1280|
2nd|Dotted half note|1440|
2n|Half note|960|
2nt|Half note triplet|640|
4nd|Dotted quarter note|720|
4n|Quarter note|480|
4nt|Quarter note triplet|320|
8nd|Dotted eighth note|360|
8n|Eighth note|240|
8nt|Eighth note triplet|160|
16nd|Dotted sixteenth note|180
16n|Sixteenth note|120
16nt|Sixteenth note triplet|80
32nd|Dotted thirty-second note|90
32n|thirty-second note|60
32nt|thirty-second-note triplet|40
64nd|Dotted sixty-fourth note|45
64n|Sixty-fourth note|30
128n|One-hundred-twenty-eighth note|15

The Note Values are the text that Max recognises as arguments in supporting objects. The only problem is that Note Values aren’t very descriptive to the end user and in relation to this particular device, the **tapout~** object does not respond to notevalues - only time given in milliseconds.

Look at the patch screenshot below:

![d02](/assets/img/d02.png)

The **translate** object converts one time format into another depending on the arguments provided by the user; in this case the conversion is from **notevalues** to **ms** (milliseconds).

The top **live.menu** is populated with time divisions (**note ratios**) that are recognisable to most users i.e 1/16, 1/8T and so on. It is connected to another **live.menu** that is populated with the Max-recognisable **notevalues**. These two objects are connected so that when the user selects the first item in the top **live.menu** (note ratios) it also selects the first item in the bottom **live.menu**. The selections match up with each other due to both being populated by the use of a **Prototype** which is a preset for a particular object in Max.

To configure the **live.menu** objects, right-click to bring up a menu, mouse to **Prototype** and select **noteratio** or **notevalue** as required.

Note that the middle outlet of the bottom **live.menu** is being connected to the **translate** object; this is because the first outlet of **live.menu** outputs the item index of the list (i.e 0 is the first item, 1 is the second and so on). The middle outlet sends the actual text for the item selected which can be used in this this case for the conversion.

Now there is a way of selecting the time divisions for the delay that is readable and tempo-related.

Note that the second **live.menu** can be left out of the presentation and hidden.

Feedback
Referring back to the delay diagram from earlier, it is easy to see where the feedback loop lies in the signal chain.
/delay_diagram1.png

After the signal has passed though the delay line (in the patch this would be the **tapin~** and **tapout~** combo), it is sent back into the delay line again with a gain control for the level.

This can be emulated by using the ***~** object before the signal returns to **tapin~**:

![d03](/assets/img/d03.png)*d03*

Note that there is an argument of **0.5** as a default for the gain control ***~**.
If a signal is now passed through the device the repeating echoes should be audible. If a singular hit was played it would be delayed and then heard, then heard again but at half of the volume from the **~ 0.5**) then heard again at a 1/4 of the original gain, then 1/8 and so on. This is due to the accumulative effect of the feedback loop processing.

As the gain here is 0. to 1., it is possible to use the right-outlet of a **live.dial** for this parameter control:

![d04](/assets/img/d04.png)*d04*

#### Mix Controls
The last parameter to add in at this stage is a means of controlling the balance between the dry (original) audio and the delay line (wet). For most pedals and outboard effects, this is achieved by a single dial that adjusts the level of the wet/dry to achieve a blend of both at different amounts.

There are a number of ways to achieve this, but in this example a new object (technically a subpatch) will be used instead for quickness. Add to the patch the **M4L.Bal2~** object and note that when mousing over the inlets and outlets there is no information supplied. The only way to see what this object requires and does it to hold down **cmd** and double-click it with the mouse to open it up like an encapsulated object from before.


![Mix Control](/assets/img/m4l.bal2.png)*m4l.bal2~*

This object is slightly over complicated for what it does and could do without some of the stages fo processing. What is of interest is the labelling of the Inputs and Outputs and the Mix control values. From this info; the first two inlets are for the first input signal and the 3rd and 4th for the second input signal, the 5th inlet is for the control of the mix and runs from a value of 0. to 100. as floats. Taking this into account connect the object up as follows:

![d05](/assets/img/d05.png)*d05*

Note that a live dial has been added to control the mix value itself with the following changes made in the inspector:

Parameter|Setting
|:-|:-|
ShortName|Mix
LongName|Mix
Range/Enum|0. 100.
Unit Style|Float

Remember that as the value is not from 0. to 1. the right outlet of the **live.dial** is not in use this time. With the initial delay working, the UI elements can now be added to the Presentation Mode.



### Topologies
There are number of different types of delay types on the market and with this basic delay up-and-running it is possible to re-use parts of the code to quickly prototype other options.

#### Stereo Delay
For example, to make a stereo variant with independent delay-times onto left and right the patch can be made as follows:

 ![d06](/assets/img/d06.png)*Stereo Delay*

All that has really changed is that the 'delay-line' and time controls have been duplicated and are being fed by the left and right signals from **plugin~** instead of being summed together. The Feedback control however is now controlling the feedback for both sides but there could quite easily have been a control for feedback for each side.

#### Ping-Pong Delay
The audible characteristics of a 'ping-pong' delay are that it is a stereo effect where the listener hears a delay 'tap' on the one speaker then again on the other. It can sound like a mono delay where the delays are being panned but in most cases the effected is produced from two separate delay-lines as follows:

![Ping Pong Delay](/assets/img/d07.png)*Ping Pong Delay*

Form a patching perspective, the incoming signal is converted to mono (***~ 0.5**) and fed into the 1st delay line; this initial delay is heard out of the left output to provide the 'ping'. The output from that same delay line is also feeding into a second delay-line which is connected to the right output to give us the 'pong'. The key here is that to maintain sync between the left and right outputs, both the left and right delay lines have the same length of delay configured. In the example below the delay time is set to 1/16th.

1. A signal is played through the effect and the user hears the dry signal
2. The first delay tap is heard on the left 1/16th after the dry
3. The second delay tap (Pong) is heard 1/16th after the first delay tap (Ping) or 1/8th after the dry signal occurred

As with the previous delays there is a feedback loop, and in this instance it occurs from the second delay line and feeds into the first.


### Limiting
One of the problems with delay effects is that the feedback parameter can cause eventual boosts in level which although can result in pleasantly saturated sounds in the tape-world, can be problematic in DAWs or digital systems. Levels can clip digitally within the device and within the parent DAW. In order to prevent this from happening in this device, and to an extent mimic the compression and saturation from tape delays, a limiter will be introduced into the patch. Although possible to design and build a limiter from scratch, this example will use the built in limiter: **omx.peaklim~**.

The limiter can be placed at various points and these should be experimented with, in these notes it will be placed between the delay-line(s) and the mix control stage.


![omx.peaklim~](/assets/img/d08.png)*omx.peaklim~ between delay lines and mix*


Two options will be shown though for where the feedback line comes from. In the example above, the feedback is taken from the first **tapout~** as before. In the example below, the feedback is taken after the limiting stage.

![d09](/assets/img/d09.png)*Feedback taken from after the limiting stage*

In terms of what is audible, try both connection types and listen for any differences in the way that audio sounds when pushing the feedback.



### Cordless Connections
The patch is becoming larger at this point with numerous points of control via UI elements. As this is still a fairly basic patch, this is a good point to look at including the [cordless send and receive objects](/routing/#cordless-connections)

By working with cordless connections, it becomes possible to move the UI elements out of the way of the patch making it more easy to follow. Look at the example below of the existing patch:

![d11](/assets/img/d11.png)*Delay patch with cordless connections*


### Feedback Loop Processing
The final stage of process for this basic delay is to add in a filter from the previous section (or an edited version of it to reflect the mono-signal input of this device). This version uses an encasulated version of the L01 filter.

Where the filter is positioned within our patch's signal flow will have a pronounced effect on the processing outcome. There are two main positions to consider; between the ***~ 0.5** and the first **tapin~** see below:


![d12](/assets/img/d12.png)*Filter outside of Feedback Loop*

or in the same position but with the return from from feedback loop also being fed into it:


![d13](/assets/img/d13.png)*Filter inside feedback loop and on input stage to delay line*

### Further Research

[Roland Space Echo](https://en.wikipedia.org/wiki/Roland_RE-201): Classic delay unit

[Strymon Timeline](https://www.strymon.net/products/timeline/): Multi Function Delay Pedal

[Creating & Using Custom Delay Effects](https://www.soundonsound.com/techniques/creating-using-custom-delay-effects): Sound On Sound Article
