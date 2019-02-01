---
title: Routing
layout: content
category: Max
order: 3
permalink: /routing/
summary: Routing messages, signals, bypassing, selecting
lastupdate: 29-11-2018
---

# Section Overview
There are a number of methods of routing within Max; direct point-to-point connections, 'cordless', dynamic; one-to-many, many-to-many, and so on. This section covers each with examples and a context.

---

## Many-to-One

### selector~ / switch

The selector~/selector object permits a 'many-to-one' signal selection. The arguments for the object dictate how many inputs there are and which is being passed through the object upon load. i.e `selector~ 4 1` would create 4 inputs with the first input signal being passed through. Note that there will be 5 inlets in the object with the first being reserved for the selection choice.

[![Signal Routing with `selector~`](/assets/img/asp_menuSelector.png)*Signal Routing with `selector~`*](/assets/img/asp_menuSelector.png)

The selection occurs when an integer is sent to the first inlet. With the `selector~ 4 1` example, sending '1' to the object lets thorugh the first, and so on. Sending a '0' however will block all signals from passing through.


#### Common Errors

- Forgetting the `+ 1` after the selection method i.e `live.menu` > `+ 1` > `selector 4 1`
- Connecting an input into the first inlet as it is reserved for the routing selection.

---

## One-to-Many

### Gates

The `gate~` object, and its counterparts, are the 'one-to-many' equivilent of `selector~`. The arguments are similar i.e `gate~ 4` would create a gate with four outlets for the single input.

---

## Many-to-Many

### matrix~


---

## Cordless Connections

Concept overview

`send~` & `receive~`

`send` & `receive`

![Cordless Connections](/assets/img/d10.png)*Cordless Connections*

#### Common Mistakes and Errors
The most common problems with using cordless connections are typos:

- Including spaces in the name of the send and receives;
- Mis-typing the `---` requirement
- General spelling

Losing connections; double-click on the send or receive to see a list of all the related 'connections'; clicing on a connection from the list will highlight where the signals ends.

---

## Dynamic Cordless
Notice that the `receive~` and `receive` objects have a top-left inlet. This is used to 'over-ride' the allocated 'cordless' source.

---
