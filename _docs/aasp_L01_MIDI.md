---
title: MIDI Summary
layout: content
category: aasp_lessons
order: 1
permalink: /midi/
summary: An overview of the MIDI specification
lastupdate: 20-12-2018
---

# What Is MIDI?

MIDI, (Musical Instrument Digital Interface) is a technical standard developed to permit the communication between digital musical instruments, computers, tablets, and smartphones. The specification comprises three main elements:

1. The MIDI communications protocol (MIDI Data)
2. The MIDI digital interface (MIDI Interface)
3. The MIDI electrical connectors (MIDI Cable Connectors)

MIDI data is composed of *event messages* that specify notation, pitch, velocity, vibrato, panning, and clock signals (which set tempo). This data can be transmitted through a [MIDI cable](https://upload.wikimedia.org/wikipedia/commons/0/02/Midi_ports_and_cable.jpg) (via a MIDI interface on a computer)to be recorded, edited and playback on a DAW sequencer. A single MIDI cable can transmit up to sixteen channels of data, each of which can be routed to a separate hardware device, or software instrument. Alternatively, as was its original intent, a MIDI device can 'play' other devices such as sound modules, other synths etc. Although the original connectors for MIDI were 5-pin DIN, it is now common to see a range of different cables and connectors being used to connect devices.

## Connectors

* USB & Firewire
* Ethernet and Internet (RTP-MIDI[^RTP])
* Wireless (Bluetooth, WiFi, [OSC](http://opensoundcontrol.org)[^OSC])
* TRS

Before the development of the MIDI standard, communication between electronic instruments from different manufacturers was either limited or simply not possible. MIDI technology was a success in that it standardised a way for devices to communicated clearly with each other.

Standardised in 1983, the MIDI protocol continues to be maintained and developed by the [**MIDI Manufacturers Association**](https://www.midi.org).  In 2016, the MMA established the **MIDI Association** (TMA) to support a global community of people who work, play, or create with MIDI.

## Specifications
There are a number of technical specification that have been developed over the years all relating to MIDI:

[MIDI 1.0 Specification](https://www.midi.org/specifications-old/category/reference-tables)

[Standard MIDI Files](https://www.midi.org/specifications-old/category/smf-specifications)

[General MIDI](https://www.midi.org/specifications-old/category/gm-specifications)

[Downloadable Sounds](https://www.midi.org/specifications-old/category/dls-specifications)

[MIDI Transport](https://www.midi.org/specifications-old/category/transport-specifications-and-info)

[eXtensible Music Format](https://www.midi.org/specifications-old/category/extensible-music-format-xmf)

[Mobile MIDI & Ringtones](https://www.midi.org/specifications-old/category/mobile-midi-ringtone-specifications)


## New Developments
There are number of new developments in MIDI, with one of the most interesting being **MIDI Polyphonic Expression**[^MPE] is a recent development in that  it brings a new means of expression (or interaction) beyond the standard MIDI spec:

> MPE is a method of using MIDI which enables multidimensional controllers to control multiple parameters of every note within MPE-compatible software.
> In normal MIDI, Channel-wide messages (such as Pitch Bend) are applied to all notes being played on a single MIDI Channel. In MPE, each note is assigned its own MIDI Channel so that those messages can be applied to each note individually.

## Useful Links

[MIDI Reference Table](https://www.midi.org/specifications-old/category/reference-tables)

[MIDI Association](https://www.midi.org)




[^MPE]: https://www.midi.org/articles/midi-polyphonic-expression-mpe
[^RTP]: https://www.midi.org/articles/rtp-midi-or-midi-over-networks
[^OSC]: Open Sound Control (OSC) is a protocol for networking  multimedia devices for purposes such as musical performance or show control.
