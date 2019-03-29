---
title: Encapsulation
layout: content
category: Max
order: 1
permalink: /encapsulation/
summary: Working with sub-patches
lastupdate: 03-12-2018
---

## Encapsulation
Max patches can regulary become busy and hard-to-follow as they increase in complexity. One method of dealing with this is the concept of *encapsulation*, or, the creation of sub-patches within a patch.

An example would be a complex LFO being 'encapsulated' from many objects and connections down to a single `p LFO` object. The screenshot below shows a basic LFO on the left with the encapsulated version on the right.

[![Encapsulation LFO Example](/assets/img/enc_01.png)*Encapsulated LFO Example*](/assets/img/enc_01.png)

Encapsulation free's up a lot of space making it easier to navigate a patch and also makes it easy to duplicate 'blocks' of code within a patch. In this example, any time the patch would require an LFO, the `p EncapsulatedLFO` sub-patch can simply be duplicated.

---

### Encapsulating
Creating a sub-patch is straightforward; select all of the objects to be placed inside the sub-patch (in the majority of cases, non-UI objects) and from the Max menu, select **Edit > Encapsulate**. The contents will be collapsed into a single object that begins with `p` to indicate this is a sub-patch. Any elements outside of the previous selection will be connected to automatically generated and numbered `inlet` and `outlet` objects within the patch.

[![Encapusulating LFO](/assets/img/encap_03.gif)*Encapsulating LFO*](/assets/img/encap_03.gif)

>Note that in this example, the process was selecting all objects within the patch first, then de-selecting those objects that were to remain *outside* of the patch by **shift+clicking** them. Then the shortcut for encapsulation was used.

---

#### Editing
To open an encapsulated patch, lock the patch and double-click it. Alternatively, hold down **Cmd** and double-click.

#### Inlet/Outlet Info
To provide information on the inlet and outlets of the sub-patch similar to when hovering over standard objects in Max:

1. Select the inlet or outlet object within the subpatch
2. Open the **Inspector**
3. Under the **Behaviour** section look for **Comment** and enter the information to be shown when hovering over the inlet/outlet


[![Inlet Naming from Inspector](/assets/img/enc_04.png)*Inlet Info from Inspector*](/assets/img/enc_04.png)


In this example, when hovering over the inlet, '*Waveform Select*' is shown in the pop-up.

---

### Issues
Some re-arranging of the `inlet` and `outlet` objects may be required to re-order the connections. This can be done by clicking and dragging the inlets or outlets within the sub-patch left and right - the number will change to indicate the order. If there are missing inlets these can be added by creating a new `inlet` or `outlet` object.

The gif below shows a new inlet being added for the triangle wave 'slope' control and the automatice re-numbering that happens:

[![Inlets](/assets/img/encap_02.gif)*Inlets and Re-arranging*](/assets/img/encap_02.gif)


### Common Errors
- UI elements left inside the encapsualted sub-patch
- Long-names for thos elements not being adjusted (automation envelopes in Live would have a lot of similarly named parameters)

---
