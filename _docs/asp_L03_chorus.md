---
title: Chorus Effect
layout: content
category: asp_lessons
order: 2
permalink: /chorus/
summary: Creating chorus-related effects
lastupdate: 03-12-2018
---

# Overview
The chorus effect is a time-domain based effect that when applied to a signal gives the sound a 'layered' or 'thickened' sound. The sound of a chorus effect can occur naturally in choirs or orchestras where there can be multiple instruments producing the same sound. The effect itself comes from the subtle variations in pitch and time that occur from different performances and performers.

In recording and mixing, the same effect can be produced by layering multiple recording recordings of the same material.

## Chorus Theory
A simple chorus can be created by modulating the delay-time paramter of a delay patch. When the time parameter of a `tapin~`/`tapout~` based delay patch is adjusted, the sound will pitch up or down as falling behind and returning to the original delay time. When this 'wobbling' sound is combined with the dry signal, the effect is the 'shimmering' or pulsing sound associated with the chorus sound.

> This lesson requires that the [delay](/delays) and [LFO](/lfos) lessons have been followed.

## Topologies

As with the delay effects, there are a number of different topologies that can be applied for a chorus effect. Each will give different qualities textures to the effect. 

### Mono

This first example below shows the effect theory in a very simplistic mono-chorus.

[![Basic Mono Chorus](/assets/img/ch_01.png)*Basic Mono Chorus*](/assets/img/ch_01.png) 

This patch contains two signal streams; the dry direct signal from `plugin~` and the processed mono signal that branches off from `plugin~` and passes through the `tapin~`/`tapout~` objects. The two streams are blended together with the standard `M4L.bal2~` object.

The modulation of the delay parameter signal (green) comes form the LFO (light-blue) and is fed into the delay section (purple).

>Note that this LFO is non-synched, non-tempo relative and  unipolar. (see the [Modulation](/lfos) section for a reminder)...

What is happening in the patch is that the delay time paramater is being modulated by the LFO output so that it constantly fluxuates. Due to the way that the `tapout~` object is coded, when the delay time is changed it can cause a pitch change if the transition from one value ot the next is slow enough. The settings shown in the screenshot will give a basic in-offensive mono chorus effect. Adjust the Mix dial to hear the effect/dry balance.

To hear the pitch/delay effect exagerated for a clearer understanding of the effect, change the parameters as follows:

- Delay / Offset: **5.**
- LFO Rate: **1.**
- LFO Depth: **25.**
- Mix: **100**

You should hear the sound speeding up and down and changing in pitch accordingly. When the Mix paramater is set to 50, the 'drift' in pitch and timing from the original is clearly audible.

### Psuedo-Stereo
By changing the mono delay circuit above into a ping-pong circuit, the chorus can also be turned easily into a psuedo-stereo effect:

[![Psuedo-stereo Chorus](/assets/img/ch_02.png)*Psuedo-stereo Chorus*](/assets/img/ch_02.png) 

What is happening here is that the first delay-line (the one being modulated) is being sent out to `M4L.bal2~` object as before, but also being sent into a second delay-line (yellow cable) that is un-modulated but shares the same delay time from the **Delay/Offset** control.

There should be a stereo quality to the chorus effect, however this is not a true stereo chorus

### Real Stereo
Another topology option would be to have either the modulated signals on the left and right modulate inversely to each other. This requires inverting the output form the LFO so that as one delay paramater is falling the other is rising.

The image below shows that by offsetting the output from `phasor~` by 0.5, it will be 180deg out of phase with an unadjusted output. 

>Note that even though `cycle~` expects a phase from 0. to 1. if the range it receives it beyond that the values are simply wrapped. In this example, with the offset, the range is 0.5 to 1.5.

The red waveform is the 'offset' waveform and the green is the standard output. 

[![Phase Inversion](/assets/img/ch_03b.png)*Phase Inversion*](/assets/img/ch_03b.png) 

To implement this in a patch;

[![Stereo Chorus](/assets/img/ch_03a.png)*Stereo Chorus*](/assets/img/ch_03a.png) 














