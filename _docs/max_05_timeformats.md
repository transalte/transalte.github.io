---
title: Max Time Formats
layout: content
category: Max
order: 1
permalink: /timeformats/
summary: Details on how Max deals with timing formats
---

# Max Time Formats

**Max Time Formats** are how Max describes time and there are two areas to consider:

## Time Formats

**Fixed Time Values** - those which stay the same regardless of the tempo of the track ie. Milliseconds; Hours/Minutes/Seconds; Samples; Frequency (Hz). For example, a millisecond is always a millisecond if the tempo is 147 or 98BPM.

and


**Tempo-Relative Time Values** - lengths of time which will change accordingly in relation to the tempo ie. Bars/Beats/Units (BBU); Ticks; Note Values. For example, 1 bar at 3BPM is obviously longer than 1 bar at 300BPM. Also, when using tempo-related time values, if the BPM of a project changes, so will the timing of the objects, i.e if a delay is controlled with BBU or note values, when the tempo changes, the delay times will change along with it to reflect the change of tempo.

Notevalues are one of the most common tempo-relative time formats, the table below shows the syntax and what each symbol corresponds to:

| Note Value | Tempo |
|:--|:--|
1nd | Dotted whole note |
1n | Whole note |
1nt |Whole note triplet|
2nd|Dotted half note|
2n|Half note|
2nt|Half note triplet|
4nd|Dotted quarter note|
4n|Quarter note|
4nt|Quarter note triplet|
8nd|Dotted eighth note|
8n|Eighth note|
8nt|Eighth note triplet|
16nd|Dotted sixteenth note|
16n|Sixteenth note|
16nt|Sixteenth note triplet|
32nd|Dotted thirty-second note|
32n|thirty-second note|
32nt|thirty-second-note triplet|
64nd|Dotted sixty-fourth note|
64n|Sixty-fourth note|
128n|One-hundred-twenty-eighth note|

---

## Conversion

Depending on the project, it may be necessary to have a user select a time in a tempo-relative format but the object being controlled only accepts milliseconds as a valid input format. This can happen when using `tapin~` and `tapout~` for a delay patch. To convert from one time-format to another the main object is `translate`:

The `translate` object has two arguments; the incoming format and then the format to be outputted. Relating to the example given above, the user wishes to select a delay-time of 1/8th triplets (8nt in notevalue syntax) but the `tapout~` object only responds to milliseconds. The syntax for the `translate` object would be as follows:

```
translate notevalues ms
```

In the context of a delay patch, the object would be configured and connected as follows:

[![Translate Example](/assets/img/timeformats_02.png)*Converting Notevalues to Milliseconds*](/assets/img/timeformats_02.png)


It is possible to convert between any timeformat which can create added flexability for creating devices. A few examples:

- A synth where the envelope for the amplitude can be selected in tempo divisions instead of milliseconds to provide tighter sound design in regards to timing
- A compressor or gate whereby the attack and release times are in tempo format instead of milliseconds
- Delay times in Bars Beats and Units instead of 1/4, 1/8th etc.

---

## Further Conversions

The patch below shows a few examples of conversions using multiple `translate` objects:

[![Time Format Conversion](/assets/img/timeformats_01.png)*Time Format Conversion with translate*](/assets/img/timeformats_01.png)

---
