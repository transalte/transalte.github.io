---
title: LFO Modulation
layout: content
category: Max
order: 1
permalink: /lfos/
summary: Building LFO's for modulation
lastupdate: 03-12-2018
---

## Overview
Signals and parameter values can be controlled and manipulated automatically in realtime through *modulation* instead of using automation or manually changing a control by hand.

A simple example of this can be found in a tremelo effect whereby the gain of a signal is manipulated in real-time (modulated in other words) via an LFO (Low Frequency Oscillator) to rise and fall rhythmically. The standard controls over the volume in such an effect would be the shape of the movement (the selected waveform), the speed of the movement (rate) and the intensity (the depth).

A similar example would be an *auto-pan* effect whereby the pan position of a sound appears to move from one channel to the other and back again automatically.

> An LFO can be thought of as an internal perpetually running automation envelope within an effect.

---

## What is an LFO?

Similar to the audible oscillators found in synthesisers, LFOs will generate waveforms but generally run at speeds much slower and below the audible range - less than 20Hz is common.

>The LFO itself is not heard - only its influence on a parameter.

The key parameters of an LFO are as follows:

* **Rate** - The time it takes for a cycle to complete (Hz)
* **Amplitude** - The range of the waveform
* **Waveform** - Shape of the generated waveform (sine, triangle, square, saw etc.)
* **Phase** - Where the start of the waveform lies within the cycle, does not have to be at 0.

The **rate** paramater dictates how many full cycles of the waveform happen in a single second, i.e a 1Hz setting will take one second to complete a full cycle of the waveform.

The **amplitude** of an LFO usually dictates how strong or subtle the modulation of the parameter will be when applied.

The selected **waveform** will control the shape of the ‘movement’ being applied to the parameter.

The most basic waveform, or shape, is a simple sine wave. Other waveforms commonly used in sound generation, and for modulation purposes, are triangle, square and saw:

[![Common Waveforms](/assets/img/waveforms.png)*Common Waveforms for Synthesis*](/assets/img/waveforms.png)



This section will cover how to create a range of different waveforms, all of which will allow for different rhythmical patterns for modulation purposes.

---

## Creating the Waveforms

The most basic LFO set-up can be created by connecting a `flonum` (or `live.dial`) to a `cycle~` object for controlling the rate of a waveform:

[![Basic LFO](/assets/img/lfoBuild_00.png)*Basic LFO*](/assets/img/lfoBuild_00.png)


However, it is more useful to set-up a 'master-clock' that will drive all of the waveforms to keep them synchronised.

To create a 'masterclock', the object to add to the patcher is `phasor~`. The help file for `phasor~` states the following:

>"Use the phasor~ object to generate sawtooth waves suitable for sample-accurate control and timing tasks... the rate can be set by frequency (Hz), or as an interval using the tempo-relative Max time format syntax"

This means that the object will work either by feeding it timing information in hertz, or alternatively, tempo-related values. As well as the `phasor~` object permitting tempo-related rates it can also be locked to the transport in Live meaning that the LFOs will always begin the start of their cycle when the transport starts allowing for repeatable modulation patterns.

The screenshot below displays the core of the LFO patch about to be built:

[![LFO Core](/assets/img/lfoBuild_01.png)*Core Section of LFO Waveforms*](/assets/img/lfoBuild_01.png)

### Saw

To create a sawtooth waveform, the raw output from the `phasor~` object can be utilised. The object below showing the waveform is `scope~` and is connected directly to the outlet of `phasor~`.

The object to the right of the `scope~` is `number~` and displays the numerical value of the amplitude of the signal.

The object with the '2.' is a `flonum` and is controlling the rate of the `phasor~` output. It can be brought in either by pressing **f** or **n** for new object and typing it's name.

[![Sawtooth Wavform](/assets/img/lfoBuild_02.png)*Sawtooth from phasor~ Object*](/assets/img/lfoBuild_02.png)

>Note that the default amplitude range of the `scope~` runs from -1. to 1. and that the amplitude of the `phasor~` output runs from 0. to 1. This explains why the saw waveform is only in the top half of the scope.

### Sine
The `cycle~` object is a sinusoidal waveform generator. There are two inlets for control; the left inlet is utilised for receiving the rate in **Hz**. The second inlet is to set the *phase* of the waveform, in otherwords, where in the waveform's shape the current output is. In more other-words, as the value going into the right inlet travels from 0. to 1., the cycle of the sine shape is generated accordingly, 0 being the start of the curved output and 1. being the end of the cycle! If the value coming in was a static 0.5, the `cycle~` object would also stop at halfway through its waveform’s cycle.

As the output from `phasor~` runs from 0. to 1. this can be used to crank out the sine wave continually:

[![Sine Wave from cycle~](/assets/img/lfoBuild_03.png)*cycle~ object driven by phasor~*](/assets/img/lfoBuild_03.png)


### Triangle
Similar to the `cycle~` object, `triangle~` can also be driven by `phasor~`. There is however an additional argument for `triangle~`.

In the example below, the argument is '0.5' - the argument itself is described as '**peak-value-phase-offset**'; a more obvious description of it's function could be '**slope**'. In the exampe below, adjusting the value sent (from 0. to 1.) to the right inlet will override the `0.5` and the result will be the waveform 'morphing' between a positive saw shape to a triangle then to a negative saw shape.

[![Triangle Waveform](/assets/img/lfoBuild_04.png)*Triangle Waveform*](/assets/img/lfoBuild_04.png)

### Square
There is an object specifically for generating square / rectangle waveforms called `rect~`, however, it functions better within the audio range for synthesis due to small spikes in the waveform at each positive-to-negative transition.

[![Waveform from rect~](/assets/img/lfoBuild_05b.png)*rect~ Output*](/assets/img/lfoBuild_05b.png)

A 'purer' square waveform can generated using a simple math object on the `phasor~` output; the 'greater-than' object `>~`:

[![Square Waveform](/assets/img/lfoBuild_05.png)*Square from >~*](/assets/img/lfoBuild_05.png)

The ‘greater-than’ object compares an incoming signal to a value (in this case 0.5), and if that incoming signal’s amplitude is greater than **0.5** (in this example), it will output a signal with an amplitude of **1.**. When the incoming signal falls below **0.5** it outputs a **0.**.

As `phasor~` amplitude ranges from **0.** to **1.**, a binary (0 or 1) signal is generated depending on where the phasor output is in it’s cycle.

Another common feature of square waveforms is the ability to change the length of the pulses. Similar to the ‘peak-value-phase-offset’ of the `triangle~` connecting a `flonum`, or any other float generating object (`live.dial` or `live.numbox` for example), can be connected to the `>~` object as follows:

[![Pulse-width Control](/assets/img/lfoBuild_06.png)*Pulse Width Control*](/assets/img/lfoBuild_06.png)

### Sample & Hold
The 'Sample & Hold' waveform is similar to the square wave in terms of its rigid shape with the exception that the amplitudes are random instead of a simple 0. or 1,; this is what gives the waveform it's 'stepped' appearance

The waveform itself is generated through taking regular amplitude readings of a signal (in this case noise) and outputting them. The 'randomness' of the amplitudes here comes from sampling the `noise~` object.

[![Sample & Hold Patch](/assets/img/lfoBuild_07.png)*Sample & Hold Generator*](/assets/img/lfoBuild_07.png)

In the image above, the key objects are `sah~` (for the sampling) and `noise~` which is the source of the amplitude readings. Similar to the method of creating the square waveform, the `sah~` object also utilises a **0.5** argument. In this case, once the signal going into the right inlet of `sah~` goes above **0.5** a reading will be taking of the signal coming into it's left inlet. This 'trigger' signal to be connected to the right inlet will come from `phasor~` meaning that the timing of when the readings take place will be syncronised with the other waveforms.

### Random
A random waveform is produced by the `rand~` object; specifically the object generates random values between -1. and 1. and interpolates between them.

The only inlet on the `rand~` object is for setting the rate at which the random values are generated. Therefore, there is no need for the `phasor~` object:

[![Random Waveform](/assets/img/lfoBuild_08.png)*Random with rand~*](/assets/img/lfoBuild_08.png)

---

## Matching Amplitudes
A brief look at the `scope~` readout for the previous waveforms shows that the amplitude of the saw and square waveforms run from 0. to 1. while the rest range from -1. to 1.

[![LFO Core](/assets/img/lfoBuild_10.png)*Matched LFO Waveform Outputs*](/assets/img/lfoBuild_10.png)

Using some basic maths objects this can be addressed so that the output from the LFO patch will be uniform.

### Saw Adjustment
The screenshot below shows how the amplitude of the saw, from `phasor~`, is manipulated to run from -1. to 1. Each stage of the process has a `scope~` connected to visualise the effect of the math objects on the signal. Pay attention to the amplitudes.

[![Adjusting Saw Output](/assets/img/lfoBuild_09.png)*Math adjustments for phasor~ output*](/assets/img/lfoBuild_09.png)

The first `scope~` shows the raw output from the `phasor~` with it's amplitude range of **0. to 1.** The goal is to have the output running from **-1. to 1.** If the signal was to be multiplied by **2.** the range would be **0. to 2.** so to counter this, the level is 'shifted' down, given an offset, by **deducting 0.5** from it to change the range to **-0.5 to 0.5.** (see middle `scope~`). Then this signal is multiplied by **2**. it produces the required **-1. to 1.** range.

|Amplitude|-0.5|-0.5 *2.|
|-|-|-|
|0.(min) to 1. (max)|-0.5(min) to 0.5(max)|-1.(min) to 1.(max)|

### Square Adjustment
As the output from the square wave is also **0. to 1.** then the same math is required for making the range **-1. to 1.**:

[![Adjusting Square Output ](/assets/img/lfoBuild_11.png)*Square Wave Output Adjustment*](/assets/img/lfoBuild_11.png)

Again, notice that the `-~` moves the waveform down, and the `*~` amplifies it.

[![Square Adjustment Detail](/assets/img/lfoBuild_11b.png)*Square Adjustment Detail*](/assets/img/lfoBuild_11b.png)

---


## Waveform Selection
Selection of the LFO waveform is achieved by using the `selector~` object and `live.menu`. See the screenshot below and refer to the [routing section](/routing/) for the workings.

[![LFO Waveform Selection](/assets/img/lfoBuild_12.png)*LFO Waveform Selection*](/assets/img/lfoBuild_12.png)

The `live.menu` object is populated with the names of the waveforms, the names are entered into the **range/enum** section in the **Inspector**:

```
SAW SIN TRI SQR SAH RND
```

---

## LFO Rate Control

The default rate, or speed, of an LFO is generally set to be in Hertz. However, other formats can be more useful. So far, a value in Hz has been used to dictate the rate of the `phasor~` output for driving the rest of the oscillators. The `phasor~` object also recognises and responds to *notevalues* and *BBU* (Bars/Beats/Units). The screenshot below shows how a default value for the speed of a `phasor~` can be 'baked-in' in both formats:

[![phasor~ arguments](/assets/img/lfoBuild_13.png)*Arguments for phasor~*](/assets/img/lfoBuild_13.png)

In this example, both phasors are working at a rate of a quarter note in relation to the tempo: **4n** on the left for **Note Values** and **0.1.0** on the right in BBU - **0 bars.1 beat. 0 units**.

Notice the ‘@lock 1’ text in the `phasor~` object.

>This is not an argument, but an attribute - attributes always have an ‘**@**‘ symbol before them.

This attribute locks the `phasor~` to the transport of the host. In the case of Max For Live, the host will be **Live** itself. This means that that the `phasor~` object will restart its output from 0 (the lowest point in the slope) when the user presses play on the transport controls within the Live session.

With this configuration, the rate of the LFO's will be tempo-related **AND** synchronised to the session.

>Be aware that while patching with this method of sync the transport has to be running in Live also in order for the LFO's to work.

### User Control
The method above shows a fixed rate being applied to the masterclock `phasor~` that is not the most user-friendly. One method of adding in a user-controllable rate in tempo-divisions is shown below and introduces a few new objects and concepts.

[![phasor~ control](/assets/img/lfoBuild_14.png)*User Controllable phasor~*](/assets/img/lfoBuild_14.png)

The `phasor~` here is set to a fixed rate of **1n** one bar and is *locked* to the transport of **Live** with the **@lock 1** attribute.

Directly after `phasor~`, there is a `rate~` object with the attribute **@lock 1**. The `rate~` object 'time-scales' the signal from `phasor~` by either speeding up or slowing down the waveform's speed, or rate, by sending it a value in the right inlet. This is the object that will allow the user to change the rate of the LFO. The **@sync lock** attribute means that when a value is received in `rate~`s right inlet, the change in speed of the LFO will only take effect when the `phasor~` signal is at 0. This means that any changes to the speed of the LFO, via rate~, will be syncronised. Therefore, when changing from a LFO speed of 1bar to 1/4 bar, the LFO output will still be in sync to the transport as the masterclock `phasor~` is in sync with the DAW.  So, `phasor~` is syncronised and locked with the DAW, and `rate~` is synchronised with `phasor~`.

The `rate~` object requires that the amount of scaling to be applied is given in a float number instead of a notevalue etc. For example, if it is sent a '2', the speed of the output would be twice as slow. When sent a 0.5, it would be twice as fast. These multiplier values are not the most user-friendly method of changing speeds to a workaround is used to convert more recognisable time formats into the multiplier required...

As the phasor~ is fixed at 1 bar, this means that each multipler value is going to be in relation that value; i.e 2 x 1bar, 0.5 x 1bar and so on. To generate this multipler from a more readable control, there are two `live.menu` objects in the patch; the top has a prototype of **noteratios** and the bottom `live.dial` has the **notevalue** prototype attached. The `translate` object converts the **notevalues** into **hz** which then pass through a `!/ 1.` object.

>The standard `divide-by` object would be `/ 2.` which would mean that the incoming value would be divided by the `2.`. The exclamation, reverses this and causes the *argument* to be divided by the incoming signal. i,e 2 / the incoming signal.

This division provides the multipler required for `rate~`. However, the last object is a `*~ 0.5` in order to match the rate exactly. See the screenshot below:

[![notevalue to rate multiplier example](/assets/img/lfoBuild_15.png)*Tempo Division to rate~ Multipler Conversion*](/assets/img/lfoBuild_15.png)

The example shows the 1st sync method at 1/4 bar and the second at the same, note that the scope outputs are identical in cycle length and syncronised to begin at the same time due the lock attribute in both.

In the context of the LFO patch, the set-up would be as follows:

[![LFO Rate Control](/assets/img/lfoBuild_16.png)*LFO Rate Control*](/assets/img/lfoBuild_16.png)

---

## Bipolar / Unipolar LFO
So far, this LFO is *Bi-polar*, in otherwords, the signals generated are both in the negative and positive ranges passing through 0. The alternative is to have a *Uni-polar* output whereby the signals stay either in the negative to zero range or the zero to positive ranges. Both can be utilised for different purposes.

[![LFO Polarity Examples](/assets/img/lfoBuild_17.png)*LFO Polarity Examples*](/assets/img/lfoBuild_17.png)

When modulating a parameter, a bipolar LFO range could be utilised to modulate a filter cutoff so that the output would sweep below the chosen frequency and above. A uni-polar LFO would be useful for a tremelo effect whereby the gain of a sound would range from unity gain to no-gain.

To provide the user with the option of choosing either method look at the patch below, specifically the math objects after the waveform select `selector~ 6 1` object.

[![LFO Polarity Selection](/assets/img/lfoBuild_18.png)*LFO Polarity Selection*](/assets/img/lfoBuild_18.png)

---

## LFO Implementation

### Parameter Modulation
The remaining control for an LFO would be the 'depth', or how intense the effect of the LFO will be on the parameter that will be modulated. In order to demonstrate this, a quick example will be given of how to apply modulation.

To apply modulation to a parameter multiply the chosen parameter's value by the LFO output and then add the resulting signal with the original again. The example below shows how a uni-polar LFO can be utilised to modulate the Cut-Off frequency of a filter (`svf~`).

[![Cut Off Modulation](/assets/img/lfoBuild_19.png)*Cut Off Modulation via LFO*](/assets/img/lfoBuild_19.png)

In this example, the modulation is implemented so that the range of the modulation will always go above (postive uni-polar direction) the chosen frequency (44.4Hz) and return back down, never going below. This is achieved by using a uni-polar LFO output in combination with adding the orginal signal with the modulated signal. Without the inclusion of the `+~` towards the end, the frequency sweep would run *from* **0 Hz** up to the chosen cut-off frequency instead.

### Depth Control
The depth control for an LFO dictates how much modulation will occor on a signal, i.e how subtle or exagerrated effect of the LFO will have on a parameter. With a depth at **0** there should be no aubile modulation taking place and the paramater value should be static.

A depth control, basically applies positive gain to an LFO signal and can be implemeted with a standard `*~` object (highlighted in yellow):

[![Depth Control](/assets/img/lfoBuild_20.png)*LFO Depth Control*](/assets/img/lfoBuild_20.png)

The range of the `live.dial` here has the standard **0. to 127** value but it could be any range.

>Note that in the example above, the `noise~` object is used as a sound source instead of `plugin~` and that the patch is only processing one side.

---

## Fixes
The LFO is mostly complete at this point for use in patchesl however there are two issues that need to be addressed.

The first is that on some waveforms, particularly those with abrupt and quick changes in amplitude (square, saw and triangle if the 'slope' is being used and the 'sample and hold'), whereby an audible click can be heard during the modulation from one value to the next. These transitions must be smoothed out to avoid the audible jump.

The second is that the modulated signal being sent can be out of the accepted range for the object parameter (in this case the svf~ may receive values in the cutoff frequency beyond 20000 Hz) and should be limited or *clipped*.

### Clipping Values
Limiting or capping values in Max can be as straight-foward as intruding the `clip~` object with arguments for the minimum and maximum values permitting to pass through. In the case of the filter example above; the configuration would be `clip~ 20. 20000.` - the signal version must be used `~`.

[![Clipping values](/assets/img/lfoBuild_21.png)*Clipping Values with clip~*](/assets/img/lfoBuild_21.png)

### Smoothing Transitions
Smoothing out the transition from one value to another can be achieved by using the `rampsmooth~` object which arguments for the *ramp-up* time (for rising transitions) and the *ramp-down* time (for falling transitions). The times are set in samples so the object can be configured as `rampsmooth~ 100 100`

This will provide a ramp time of 100 sample if the signal is moving in an upwards directions, and 100 if it is falling.

These values can be adjusted to suit the abrupt changes in direction for the saw and square waveforms as follows:

saw: `rampsmooth~ 0 100`; the abrupt change here is on the downwards drop after the steady rise so there is no need to smooth out the ramp-up time

square and 'sample and hold': `rampmooth~ 100 100`; as there is an abrupt rise *and* fall in the waveform the ramp-up and ramp-down must be given a value onther than 0.

>The exact values here are for demonstration purposes only, depending on the requirements of a device these may be too long. Adjust the times as required.

By smoothing out the transitions, there should be less audible artefacts during the modulation of paramaters.

[![Smoothing Transitions](/assets/img/lfoBuild_22.png)*Smoothing Transitions*](/assets/img/lfoBuild_22.png)

### Limiting Ranges

The next issue can be found when using the triangle waveform and adjusting its ‘point 1’ parameter ('slope' from previous descriptions).

When the user reaches the extremities of the slope with values approaching **0.** or **1.** there is an audible click similar to what was happening previously with the *saw* and *square* waveform output.

To fix this, one solution is to limit the *minimum* and *maximum* range outputted from the `live.dial` or `flonum` objects form the inspector.

Another, tidier, option is to utilise the `scale`[^1] object to adjust the range coming out from the parameter object. In the example below, a `live.dial` is being utilised to control the 'slope' parameter of `triangle~`. To prevent the audible clipping the range of the `live.dial` output is being reduced to *minimum* of **0.02** and a *maximum* of **0.98**.

The syntax for the `scale` object is as follows:

`scale (MinimumInput) (MaximumInput) (MinimumOutput) (MaximumOutput)`

For this example: `scale 0. 1. 0.02 0.98`

[![Range adjustment with scale](/assets/img/lfoBuild_23.gif)*Range Adjustment with scale*](/assets/img/lfoBuild_23.gif)

>Remember that the right outlet is being used as it generates a value from 0. to 1. regardless of the range of the dial:

[![Triangle Slope Control](/assets/img/lfoBuild_24.png)*Slope Control for triangle~*](/assets/img/lfoBuild_24.png)

This will now prevent the user from entering in the extreme values that cause clicks. However, if the user decides to automate this parameter, there will still be clicks if the jump from one value to the next is too great. To solve this problem, the messages can be converted to a signal and smoothed out again:

[![Smoothing Dial Output](/assets/img/lfoBuild_25.png)*Smoothing the dial Output*](/assets/img/lfoBuild_25.png)

To implement these same measures for the square wave:

[![Smoothing Square Output](/assets/img/lfoBuild_26.png)*Smoothing the Square Wave Output*](/assets/img/lfoBuild_26.png)


[^1]: There is also a signal version; `scale~`
