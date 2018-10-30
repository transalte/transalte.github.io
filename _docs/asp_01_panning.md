---
Title: Panning
layout: asp
category: ASP
order: 3
tag: panning, stereo, mono
---


## Panning

Panning can be achieved a number of ways in M4L, there are the **M4L.pan1~** and **M4L.pan2~** objects that come with the installation. Both versions work with messages to control the panning position which is problematic for modulation.

In the example below, the patch is much simplified and works with an audio signal controlling the panning position.

![MonoPan](images/panMono.png "CAProTools ION")

### List of Objects:
* line~
* *~
* +~
* cycle~
* inlet / outlet
* live.dial


### Compressed Code
