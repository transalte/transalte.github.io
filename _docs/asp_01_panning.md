---
title: Panning
layout: asp
category: ASP
order: 3
tag: panning, stereo, mono
---


# Panning

Panning can be achieved a number of ways in M4L, there are the **M4L.pan1~** and **M4L.pan2~** objects that come with the installation. Both versions work with messages to control the panning position which is problematic for modulation.

In the example below, the patch is much simplified and works with an audio signal controlling the panning position.

## List of Objects:
* line~
* *~
* +~
* cycle~
* inlet / outlet
* live.dial

## Patch Screengrab

![MonoPan](/assets/img/panMono.png "CAProTools ION")

## How It Works

This method of panning does not simply adjust the level of a sound being sent to either channel. The iclusion of the [cycle~] object allos the user to select the midway point of the 'panning' i.e dead centre, and the volume of each channel is sitting at a linear gain of 0.707 on either side. This ensures that the perceived volume of the audio does not get louder as it 'moves' through the stereo image. 

### Compressed Code
