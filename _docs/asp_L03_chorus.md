---
title: Chorus Effect
layout: content
category: asp_lessons
order: 2
permalink: /chorus/
summary: Creating chorus-related effects
lastupdate: 14-12-2018
---

# Overview
The chorus effect is a time-domain based effect that when applied to a signal gives the sound a 'layered' or 'thickened' sound. An 'organic' chorus effect can occur naturally in choirs or orchestras where there are multiple similar sound sources playing simultaneously. The 'thickening' effect itself comes from the subtle variations in pitch and time that occur from the different performances and performers.

In recording and mixing, a similar effect can be produced by layering multiple recordings of the same material or by using chorus pedals, outboard or plugins.

> This lesson requires that the [delay](/delays) and [LFO](/lfos) lessons have been followed.

## Chorus Theory
In Max, a simple chorus can be created by modulating the delay-time parameter of a delay patch with an LFO. When the time parameter of a `tapin~`/`tapout~`-based delay patch is constantly adjusted, the sound will pitch up and down, falling behind and catching up with the original signal. When this 'drifting' sound is combined with the dry signal, the effect is the 'shimmering' or pulsing sound associated with the chorus sound. The core concept of the effect is shown in the image below:

[![Chorus Diagram](/assets/img/ch_04.png)*Basic Chorus*](/assets/img/ch_04.png)

### Audio Examples

The example below, a dry guitar loop is heard followed by the chorus effect.

<audio controls>
  <source src="/assets/audio/chorus_gtr.mp3" type="audio/mpeg">
Your browser does not support the audio element.
</audio>

In the following synth example[^1], the chorus effect can be heard blending in with the dry over time:

<audio controls>
  <source src="/assets/audio/chorus_synth.mp3" type="audio/mpeg">
Your browser does not support the audio element.
</audio>



## Topologies

As with the delay effects, there are a number of different topologies that can be applied for a chorus effect. Each will give different qualities textures to the effect.


### Mono

This first example below shows the effect theory in a very simplistic mono-chorus.

[![Basic Mono Chorus](/assets/img/ch_01.png)*Basic Mono Chorus*](/assets/img/ch_01.png)

This patch contains two signal streams; the dry direct signal from `plugin~` and the processed mono signal that branches off from `plugin~` and passes through the `tapin~`/`tapout~` objects. The two streams are blended together with the standard `M4L.bal2~` object.

The modulation of the delay parameter signal (green) comes form the LFO (light-blue) and is fed into the delay section (purple).

>Note that this LFO is non-synched, non-tempo relative and  unipolar. (see the [Modulation](/lfos) section for a reminder)...

What is happening here is that the delay time paramater is being modulated by the LFO output so that it constantly fluxuates. Due to the way that the `tapout~` object is coded, when the delay time is changed it causes a pitch change if the transition from one value to the next is slow enough. The settings shown in the screenshot will give a basic in-offensive mono chorus effect. Adjust the Mix dial to hear the effect/dry balance.

To hear the pitch/delay effect exagerated for a clearer understanding of the effect, change the parameters as follows:

- Delay / Offset: **5.**
- LFO Rate: **1.**
- LFO Depth: **25.**
- Mix: **100**

You should hear the sound speeding up and down and changing in pitch accordingly. When the Mix paramater is set to 50, the 'drift' in pitch and timing from the original is clearly audible.

### Psuedo-Stereo
By changing the mono delay circuit above into a variation of a simple 'ping-pong' delay circuit, the chorus gain a psuedo-stereo characteristic:

[![Psuedo-stereo Chorus](/assets/img/ch_02.png)*Psuedo-stereo Chorus*](/assets/img/ch_02.png)

Here, the first delay-line (the delay being modulated) is being sent out to `M4L.bal2~` object as before, it is also being sent into a second un-modulated delay-line (yellow cable) that shares the same delay time from the **Delay/Offset** control.


### Real Stereo
Another topology option would be to have either the modulated signals on the left and right modulate inversely to each other. This requires inverting the output form the LFO so that as one delay paramater is falling the other is rising.

The image below shows that by offsetting the output from `phasor~` by 0.5, it will be 180deg out of phase with an unadjusted output.



The red waveform is the 'offset' waveform and the green is the standard output.

[![Phase Inversion](/assets/img/ch_03b.png)*Phase Inversion*](/assets/img/ch_03b.png)

>Note that even though `cycle~` expects a phase input from 0. to 1. if the values received are outwith that range the values are simply wrapped. In this example, with the offset, the range is 0.5 to 1.5 but the cycle continues regardless.

To implement this in a patch;

[![Stereo Chorus](/assets/img/ch_03a.png)*Stereo Chorus*](/assets/img/ch_03a.png)

## Further Research

[More Creative Synthesis With Delays](https://www.soundonsound.com/techniques/more-creative-synthesis-delays#top): Article with some interesting points for effect modifications

[^1]: Taken from the [real stereo](#real-stereo) example.
