---
title: Velocity Modulation
layout: content
category: aasp_lessons
order: 1
permalink: /velocity_Mod/
summary: Utilising velocity for modulation
lastupdate: 21-01-2019
---

# Overview

In addition to amplitude control, velocity is regularly utilised as a modulation source for other parameters such as filter frequency, resonance and so on. As an example to introduce more expressiveness into a synthesiser, velocity could be used to modulate the amount of saturation, or chorus that is applied to a sound according to how hard/fast a key has been pressed.

In this section, velocity will be used for filter frequency modulation but the theory and application is the same regardless of the parameters that it could be routed to.

---

## Filter Envelope

Filter envelopes work by taking a fixed value and modulating (changing that value over time). The shape of that modulation is dictated by an enveloped shape and the amount, or intensity, of the modulation is controlled by the velocity of the notes coming in. Commonly, there is an 'envelope amount' dial to further control how intense the modulation will be. The patch below shows an implementation of velocity modulation; the velocity modulation object additions have been highlighted in yellow.



### Implementation

[![Velocity Modulation](/assets/img/aasp_monosynth_12_velMod.png)*Velocity Modulation of Filter Frequency*](/assets/img/aasp_monosynth_12_velMod.png)


Velocity values are being pulled from `ddg.mono` in the same way as for the amplitude modulation including being scaled between 0. and 1. by the `/ 127.` object. The key difference here is that the filter modulation should only be triggered by a NoteOn (> 0 velocity) otherwise the filter envelope would be triggered twice for each key press (on and off).

Ideally, the filter envelope patch should simply ignore NoteOff velocities (0). This is achieved by the `sel` object. When a value passes into this object, if the value is 0 (as per the argument in this patch) then a 'bang' is passed out of the left outlet. Any other values that do not match the argument (greater than 0 i.e NoteOn velocities) are passed through the right outlet - these are the NoteOn values for this patch that will be utilised.

### Modulation Offsets

To acheive the modulation, the frequency control (`live.dial`) for the filter outputs a value that is being modulated (interfered with) by the output of the `adsr~` object. To achieve this modulation, the first thought may be to just multiply the output from the `live.dial` with the output of `adsr~` as follows:

[![Filter Envelope Mod A](/assets/img/aasp_monosynth_13.png)*Filter Envelope Mod At*](/assets/img/aasp_monosynth_13.png)

The outcome of this method would be that the modulated frequency value going into the `svf` filter would start from 0 Hz and rise up to a maximum of 150Hz (depending on the velocity used).In the majority of instruments, the filter frequency chosen is usually the minimum value that will be output i.e from 150 Hz upwards and back. To acheive this there has to be an offset added ito the patch:

[![Filter Envelope Mod B](/assets/img/aasp_monosynth_14.png)*Filter Envelope Mod B*](/assets/img/aasp_monosynth_14.png)

Now, the filter frequency will still initially run from 0Hz minimum to 150Hz maximum, but after this modulation takes place, 150 is added to the values therefore offsetting the signal so that 150Hz is the starting point of the modulation now instead of 0Hz.

> This method of modulation always follows the same proceedure: (Parameter * Modulation) + Parameter

### Modulation Amount

Even though this method of **velocity > envelope > parameter** modulation works, the modulation amount is directly related to the velocity; there is no method of attenuating or intensifying the effect. As the envelope is the driving force behind the filter frequency's values (static or otherwise), it would make sense that being able to increase or decrease the range of the envelope would provide this level of control:

[![Filter Envelope Amount](/assets/img/aasp_monosynth_15.png)*Filter Envelope Amount*](/assets/img/aasp_monosynth_15.png)
