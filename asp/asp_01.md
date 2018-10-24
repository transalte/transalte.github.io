
# Introduction to Max/Max For Live

## What Is Max For Live?
In this module, you will learn the visual programming language **Max For Live** (M4L) by building a number of devices that can be put to use in a Live session. Max is an enormous sand-box of a program with such a wide range of applications that it would be impossible to cover every detail of the 1000-odd objects and their possibilities from start to finish in a single course. The aim here is to provide you with the fundamentals of the application so that you can understand the key aspects of how Max works.

> Although the focus of this module is on **Max For Live**, some familiarity with Ableton Live will help greatly; it is recommended that you read the following information [https://www.ableton.com/en/manual/live-concepts/] to bring yourself up to speed.

The documentation that comes with Max is very detailed and provides clear examples of how the individual objects work, but at times the documentation can sometimes lack the context needed in order to fully understand how certain objects would tie in with your own ideas. By working through step-by-step examples in class and reverse engineering complete devices you will understand how the various objects and concepts within the programme work.  Some of the effects (referred to as ‘devices’ in M4L) covered in the module may be familiar, however, it is expected that students will be adding in more controls, features and flexibility than you may find in existing effects. Seeing how such effects are built will allow you start thinking about how you can expand one to meet your own needs.



## Creating a New Max For Live Device
Max For Live splits its ‘devices’ into three categories:

- **Instruments**
- **MIDI Effects**
- **Audio Effects**

In this module we will be creating Audio Effects.

1. With Live open, create a new audio track and drag in an audio clip of your choice.
2. Add a Max Audio Effect device onto the track by dragging the template from the **Live Browser**.
3. Click the edit button on the new device (third icon from the top right of the device).


This will open the **Max Editor** in a new window complete with the default contents for this type of device. This is the window where all devices are created and edited:

/io.png


## Details About Max & Max For Live
Max is classed as a visual programming language meaning that instead of building devices using raw text line-by-line (such as the C programming language), the code is represented by what are referred to as objects. Each object serves a different purpose and/or function ranging from simply adding two numbers together or performing more complex tasks such as filtering.

The two Max objects in the default window, **plugin~** and **plugout~**, provide a means of getting a signal into the device from Live and then back out again. As they are directly connected together already, the incoming L & R signals of the Live clip will simply pass out of the device unprocessed.

> It is important to notice the use of a tilde in the name of objects. A tilde in the name indicates that this is an object that processes audio signals. You should also notice the striped cables which indicate that the information travelling through them is a signal. If an object has no tilde, and/or is connected with a single-coloured cable then it functions with data of some sort which could be a list of numerical values, text, or symbols. Another point of note is that another difference between signal and data objects is that an object that outputs a signal will send the signal constantly. Data messages are transmitted generally once when triggered. This will make more sense as you progress through the various devices.

Note that while the **Max Editor** is opened, it is possible to hear the end result of any processing of a signal coming from Live in real-time.

### Saving A Device
To save a device in Max For Live, you can use the standard save shortcuts or access save from the **Max Editor** window. Once saved, your device will appear under the appropriate section in the **Live Browser** i.e under Audio Effect, MIDI Effect or Instrument.

Save your device just now from the Max menu, creating a folder to save your devices under and title it something obvious so that you can find it again. Use the same naming convention throughout i.e adding initials or text before the name of the device so they are all grouped together and easily identifiable later ie. **rg_filter_01**. The devices you create are found on your drive inside the **User Library** in the Presets folder and for easy access use the Places section of the **Live Browser** and add the appropriate folders.


### Adding Objects
There are a number of ways to add objects to **M4L** and when you are unfamiliar with the 1000-odd objects available.

The most common and quickest method is to move the mouse to a blank area of the patcher and click the mouse to bring up a blank object and begin typing in the name. Max will attempt to complete the name automatically as you enter letters in and will give suggestions through a drop down menu which can be navigated with the arrow keys.

Alternatively, placing the mouse cursor over an area where there are no objects and pressing the ’n’ key (for a new object) will bring up a blank object where you can can begin typing in it’s name.

Finally, there is also an object browser available from the left hand side of the patcher which gives short descriptions of the objects.



## Short-Cuts List
As with most software packages there are a number of shortcuts that are essential for navigating and accessing features. The shortcuts listed below are the ones that you will be using the most when developing your devices so it is a good idea to start using them from the beginning.

**b**  button
**c**  comment box
**f**  float number box (decimals)
**i**  number box (integer / full number)
**l**  new live object (lower case L)
**m** message box
**n**  new object
**t**  toggle
**x**  shows these shortcuts



# First Device: Creating a Filter Effect
The first device will be a straightforward stereo filter with selectable filter types, user controllable frequency and resonance parameters with all parameters visible and available for automation in Live.

## Stage 01: Input Meters

1. Re-open your saved patch if closed.
2. Place the mouse cursor over an area where there are no objects and press the n key. This will bring up a new object that is empty and is waiting for you to type the name of an object into it.
3. Press L and you will see Max attempt to auto-complete your entry; the object to locate is the live.meter~ object. Once you hit enter, it should appear on the patch.
4. Move the meter to the right of the plugin~ object by dragging it with the mouse.
5. Hold down the alt key on your keyboard, click and drag with the mouse to the right to create another copy.
6. Click and drag from the bottom left outlet of plugin~ to the almost invisible inlet on the top of the left live.meter~ object. Hover over this inlet for a description.
7. Do this same again but from the bottom right outlet of plugin~ to the duplicated live.meter~.
8. If you play the clip in Live you should see the meters reacting to the audio.


/live.meters.png


## Stage 02: The Filter

This next object to add is the object that performs the actual filtering of the audio signal: **svf~**.

1. Within the Max Editor, press '**n**' on your keyboard for a new object
2. Press 's' on your keyboard to start the autocomplete, **svf~** may show up immediately and you can press enter on the keyboard to add it to the patch. If it doesn't show up, you can scroll through the list of objects beginning with '**s**' with the arrow keys until you find it or type in the rest of the objects name to filter out the unwanted objects.

### Details of **svf~**

The **svf~** object has three inlets at the top and four outlets at the bottom; when the mouse pointer is placed over them a comment box will appear providing details of the inlet, or outlets, properties. In this case, hovering over the top left inlet displays the following:

> **svf~**:(signal/float) Input

This format of the inlet/outlet properties is the same for all objects within Max following the same structure:

> ObjectName:(DataFormat)(InputName/Description)

If we refer back to the inlets and outlets of **svf~**, it tells us the following:


|I/O|DataFormat|Name/Description|
|:------|------|------|
|Inlet 1|signal or float|Input|
|Inlet 2|signal or float|Cut Off Frequency(Hz)|
|Inlet 3|signal or float|Resonance (0-1)|



Translating Max into plain-speak, inlet 1 is the input for the filter and it expects either a signal (in this case it could be the audio from a synth or sample) or a **float** which is a fractional number.

> You may have noticed that some of the inlets will accept float values as well as audio signals. This adds more flexibility in the types of control you can use, i.e a static value from a dial (data) or an oscillator (audio signal).
> Another point to cover at this stage is the two types of numerical inputs in Max: Floats and Integers. Integers are full numbers i.e 1 or 47 and floats are values such as 20.5 or 100.25346. Floats will always have a decimal even if it is a full number such as 1. or 25. and even 0.

Inlet 2, controls the cut-off frequency for the filter and can be done so by an audio signal or a number (float again). Inlet 3 is for controlling resonance and has a range of 0. to 1.The range of the filter is in Hz as expected.

Looking at the outlets:

|I/O|DataFormat|Name/Description|
|:------|------|------|
|Outlet 1|signal|Low-pass Output|
|Outlet 2|signal|High-pass Output|
|Outlet 3|signal|Bandpass Output|
|Outlet 4|signal|Notch Output|

Due to the way this particular filter object is coded it is essentially four filters in one; each filter is constantly processing the incoming signal simultaneously and outputting a filtered signal. The user simply selects which filter they want to use by attaching a cord to that particular filter type's outlet.

### Recap for **svf~**
The **svf~** object takes an audio signal input (our sound(s) from Live) on inlet 1; inlet 2 allows us to control the cutoff frequency of the filter with values between 20. and 20000. and the third inlet allows us to control the resonance of the filter by feeding it a value between 0. and 1. We can choose whether to control these parameters with messages (data) or audio signals.



## Connections and Controlling Values
The **svf~** object only has one inlet for audio input meaning that is effectively a mono processor. To quickly get a proof-of-concept up and running the device will be built as a mono processor initially.

/filter02.png

In the image above, the left and right outlets from **plugin**~ are connected to the first inlet of a ***~** object before being sent into the **svf~** object. This multiplication object combines both left and right signals together and then multiplies their amplitude by 0.5 - this halves the amplitude of the signal avoiding any clipping when we hear the signal on output after processing.

> In Max, the amplitude of a signal is describes linearly i.e not in dB. When we multiply a signal by 1. (note the float) it's amplitude is unchanged; when multiplied by 0.5 it is halved. Multiply by 2. and the amplitude is doubled.

The first outlet of **svf**~ (the Low-Pass outlet) is then connected to both the left and the right inlets of **plugout**~. The means that we are only processing the left channel of the incoming audio but are hearing it out of both the left and right channels after processing.

The next stage is to add controls for the filter’s frequency and resonance parameters. There are a number of objects that can be utilised as controls for objects but during the prototype stage the quickest ones to use are the number objects; the float (shortcut **f** on the keyboard) and the integer (shortcut **i**) objects.

### Frequency
By connecting either a float or integer number box to the second inlet of **svf~** you can control the cutoff frequency by entering the value into the box (when the patch is locked), or by clicking and holding on the number box and pushing the mouse/trackpad etc. up and down. **svf~** Keep in mind that although the **svf~** frequency runs from 20-20000Hz, the number box will permits figures above and below this being sent to the **svf~**.

### Resonance
For the Resonance control, we need to be more specific about the type of number box as it runs from 0. to 1. meaning that it expects a float so that we can enter 0.004 or 0.9745. Connect a float number box here. Be very careful to not go over 1. as the resonance may damage your hearing.

/filter03.png

Once you have played an audio clip through this device and can hear the filter working save it as B00XXXXXXXX_filter. Note that the file extension for a Max For Live device is ***.amxd**.



## Controls
From an end-user point of view these number boxes aren’t the most ergonomic to use so we will change them for dials that will be similar to those found in the Ableton Live UI. The object for these is the **live.dial** object. When you add this to your patch you should find that the values on the dial go from 0 to 127 (lock the patch to try this). There are also two outlets on the **live.dial**, the left outlet spits out the data displayed under the dial, in this case a value from 0 to 127. The right outlet also spits out the data but this time scales it between 0. and 1.
Connect a **live.dial** to **svf~** as follows:

/livedial01.png

The left dial by default will send values from 0 to 127 to the **svf~**, this is fairly useless for a filter so we will fix this by changing some values the inspector.


1. Open the **Inspector** for the **live.dial**.
2. Under the **Parameters** subsection change the **Type** from **Int (0-255)** to **Float**. When set as **Int**, we are capped at values between 0-255, with **Float** this limit is removed.
3. Change the **Range/Enum** to 20. 20000. (remember the period after the numbers and the space between those two values)
4. Change the **Unit Style** to **Hertz**.
5. Change the **Short Name** to **Freq** - this changes the UI text
6. Change the **Long Name** to **“Filter Frequency”** - this is the text allocated to the envelope names in Ableton Live and as the name contains a space you must include the quotation marks


If you jump back into the main patch, the dial should now run from 20 to 20kHz albeit very hard to fine-tune the more useful lower values. To skew the distribution of the numbers across the dial, open the the Inspector again and set **Exponent** to **3.3333** - this will make the dials more user friendly.

For the second **live.dial**, connected to the Resonance parameter control, we do not need to change much apart form the **Short** and **Long Names**. Be sure that you are using the right outlet as this matches the Resonance parameter range of the **svf~**.

>For most live.x objects, the right outlet will output a range between 0. and 1. regardless of the range of the object dictated in the inspector.

You should now have working prototype of the the filter the next stage is to duplicate sections to allow for stereo.



## Stereo Implementation
Making this patch stereo is a case of duplicating or creating another **svf~** object and connecting the **Freq** and **Res** controls to both in the appropriate inlets. Note that the patch now utilises note the **L** and **R** channels coming from **plugin~**.

There could be separate dials for left and right but at this point it is best to avoid overcooking such a basic device.

/filter04.png

#### Cord Organisation
Notice that in the example screenshot the cords look a bit tidier than in your patch. In the screenshot, the cords bend around the objects which make it tidier and easier to follow. To do this, click a cord (it will go blueish) and press **cmd+y.** You can then click and grab straight segments of the cord and move it about. The colour of the cords has also been changed, this makes it much easier to follow the signal paths. This is done by selecting a cord by clicking on it (shift and click others to add to the selection), right clicking on the cord and selecting Color from the context menu.

Keeping your patches organised like this is a good habit to adopt as early as possible for ease of following signal flow and finding errors. Also, when you eventually hit a brick wall with a patch, tidying it up can lead to a moment of zen while being distracted with the drudgery of aligning objects and cords…



## Filter Type Selection
Now that the filter is working, the next step is to add in a means of selecting which filter is audible. As stated earlier, the **svf~** object has four outlets; one for each filtered signal and all four filters are running simultaneously. The object that will allow us to switch between which of these four signals is routed to the audible output of the patch is **selector~**.

/selector01.png

From the screenshot above, the **selector**~ object has two numbers after it’s name. These are arguments that ‘bake-in’ particular settings to an object - note that they can usually be overridden in some manner by the user.

In this case, the first argument dictates how many inlets the object will have (4 in this case), and the second argument dictates which of these inlets will be routed to the outlet when the patch initialises (loads for the first time).

How can you find out what arguments are available for an object?  When you type in an objects name after calling up a new blank object (**n** on the keyboard) you will see a dropdown if you press the space bar after the objects name as shown in the screenshot below or, alternatively, you can go to the help file or reference for the object for more details.

/selector02.png

### Details of **selector~**
You should notice that even though the argument is for 4 inlets there are 5; the first inlet is the 'controller' inlet for this object.

When **selector~** receives a number in the first inlet, it routes the audio from the selected inlet to the outlet. Receiving 1 lets through the first signal inlet, 2 the second and so on. Sending it a 0 however will allow no signal through effectively acting as a gate.

Once the arguments are in place, add the **selector~** objects into the patch as follows:

/selector03.png

### **live.menu**

Using numbers for the filter selection will be fairly meaningless to the end-user so a better method is to use menus which name the outlets.

Add a **live.menu** object to the patch and notice that the drop-down menu is already populated with the text ‘one’, ‘two’ and ‘three’.

/livemenu01.png

If you hover over the outlets, you will notice that the first gives us the ‘Item Index’ of the menu selection (0, 1, 2, 3 and so on) and that the second outputs the ‘Selection Symbol’ ie. the actual text.

|Selection|Item Index|Item Symbol|
|:------|------|------|
|First|0|One|
|Second|1|Two|
|Three|2|Three|

To see what actually happens when you make a selection some basic fault finding can be applied by connecting **message boxes** to these outlets - this is a good technique to remember as you will have to fault find or monitor the outputs of objects regularly. To add in the **message boxes** press **m** on the keyboard.

	There are two inlets on a **message box**, when data is connected to the first inlet, it is simply sent out out the only outlet of the object. When connected to the second inlet, the data is displayed but not sent.

Using the right inlet of the message box allows us to see the message without sending it out. In the example above, the user has selected **three** from the menu; the index for this, from the first outlet, is showing as **2** even though it is the third item in the list. This is due to the index of all the items in the menu starting at **0**. The output from the second outlet is **three**, which is the symbol or text related to that index in the list.

Combining **live.menu** and **selector**
The first step is populate the **live.menu** with the filter types. Select the **live.menu** by clicking on it once and open up the Inspector for it (press the circled **i** in the right side of the patch border). Look under **Parameter** section for **Range/Enum** and replace the **one two three** text with the type of filters coming into the inlets of selector:

	Low High Band Notch

If you cannot see the **Parameter** section, make sure that in the top section of the **Inspector** you are looking at **All** instead of Basic or **Layout** etc.

If you connect a message box to the **live.menu**'s first outlet and selected **Low** (the first item in the menu), you will see that it outputs a **0** - however when this value is sent to **selector~** it will not send any signal out (see previous notes on **selector~**). The signal inlets of the **selector~** start at 1, whereas the outputs from **live.menu** start from 0.

To get around this misalignment of values a **+** object between the **live.menu** and the **selector~** object with the argument **1**.

/menu_selector.png

	Remember two things, we are not dealing with  signal here so there is no tilde after the **+** sign.

Now, an output of 0 from the **live.menu** triggers a 1 on **selector~** which lets the lowpass through, and so on. The menu selections now match up with the filter outputs of the selector~ object:

|Menu Selection|Menu Item Index|After +1|selector~ Signal|
|:------|------|------|
|Low|0|1|Low-Pass Outout|
|High|1|2|High-Pass Output|
|Band|2|3|Band-Pass Output|
|Notch|3|4|Notch Output|



## Presentation Mode

At this stage, the patch is only controllable inside the **Max Editor**. Save your device and close the **Max Editor**. Back in **Live**, you will see a relatively un-Live user interface that most-likely has the dials and some other elements out of reach, it essentially look like the patch or part of it.

/pres01.png
Open up the device again and go into **Presentation Mode** by either clicking **drawing-board** icon at the bottom left of the patch’s window, going to **View > Presentation** in the main menu or pressing **alt+cmd+e.** See the screenshot below:

/pres02.png

You will see a blank patch. This is because we need to add our UI controls to this view of the patch.

To make objects visible in the UI:

	1. Come out of **Presentation Mode** (click the icon or shortcut again)
	2. Click on the object you want to see in the UI to select them and then right-click to bring up the context menu and select **Add to Presentation**. The object should have a red outline on it now indicating it’s now active in the **Presentation Mode**
	3. Do this for all the relevant UI controls objects. The keyboard shortcut for this is **cmd+shift+p** after selecting the object

Go back to **Presentation Mode** to see the object(s) with no patch cords visible. it is possible to reposition objects anywhere in the screen and, depending on the object, resize it with the handles at the bottom right that appear when hovering the mouse over that area.

The very final step is to make the UI visible to Live:

	1. Right-click on an empty area (make sure you have not clicked on any objects) of the patcher when in Presentation Mode and select Patcher Inspector from the context menu that pops up.
	2. Under the View heading in the Inspector, check the box to the right of ‘Open in Presentation’ and save your patch.

Back in Live, the devices UI should now contain all of the objects you added to the Presentation.

As a final addition, add a **live.gain~** object to the patch that sits between the output from the **selector~** objects and the **plugout~** as shown below; make sure that you change the **Short Name** and **Long Name** to something appropriate for the control in the **Inspector**, then add this to the UI via **Presentation Mode**

/pres03.png

In the screenshot below, some comment objects (press **c** on the keyboard to bring this up) have been added to act a labels for some of the objects.

/pres04.png

## Smoothing Controls
There may be some audible clicks or jumps when changing the cut-off frequency abruptly either manually via the dial or through envelope automation in **Live**. This ‘tearing’ sound is due to the values changing abruptly from one value to another. The way to smooth the change from one frequency to another is to glide gradually between them over a short period time, preferably short enough so that the transition is not noticeable.


To demonstrate this smoothing effect, add the following objects to your patch and connect them as shown:

- **live.dial**
- **append** 1000 (1000 is the argument here)
- **line~**
- **number~**
- **message box**

/smooth01.png

This patch will demonstrate how to smooth out the transition between values by using the **line~** object.

The **line~** object needs two pieces of information; what the next value is to go to and how long it will take to reach there. These values must be sent in that order. This is why the append object is added; it will append (add to the end) 1000 to the incoming message; in this case, as shown from the message box, the message sent to the line is "**68. 1000.** meaning that it will take **1000ms** to reach **68** sliding through all the numbers in-between.

The **number~** object after **line~** just displays the numerical values and is used in place of a message box as message boxes only work with data not signals.

When the dial is turned, or a value is entered into the dial by typing, the **number~** object should display the number gradually gliding to the next value over 1000ms. Once this has been seen, the objects can be removed from the filter patch.

In practice, change the append to a smaller value such as 15 or 25 and add the objects to the filter patch as follows:

/smooth02.png



## Encapsulation

Even though this patch is fairly simple so far, it’s starting to take up a fair amount of space in the **Max Editor**. Encapsulation allows you to create sub-patches within **Max**; these are simply patches within patches that can be easily copied and pasted and take up much less screen space. In a larger patch than this that maybe has a number of distinct sections i.e a filter, an autopan, bit crusher etc. we would aim to create encapsulated sub-patches for each of these 'sections' of the effect. For this patch, the encapsulation will be on the core sections of the filter only.

Look at the screenshot below and note the objects that are highlighted in blue now due to being selected - these are the objects that will be encapsulated:

/encap01.png

Select the same in the device being built. Note, to select multiple objects, you can drag a box around them with the mouse or **shift+click** them.

Once these objects have been selected in your patch go to the main menu and select **Edit > Encapsulate** or press the keyboard shortcut displayed from the same menu. This will create a new object beginning with **p** - this indicates the object is a sub-patch:

/encap02.png

Double click the new **p** object and you can add in a suitable name and when you hit return you should see the size shrink accordingly. Avoid spaces in the names, for this example call it **p StereoFilter**. There will be a degree of tidying-up required  at this point. Also notice that the audio inputs are split across the inputs of the patches, this is easy enough to fix and is good practice for workflow that the audio inputs are the first inlets of a sub-patch. To do this, go into **Presentation Mode** and double click on the sub-patch to open it up. Alternatively, hold down **cmd** and **double-click** on the object, the sub-patch will open and look similar to this:

/encap03.png

The objects with the numbers are the **inlets** and **outlets** of the **sub-patch**, those numbers give the order that they appear outside of the object. In this example, we can see that the **svf~** objects are being fed by **inlets** **2** and **5**. If you grab and move these inlet objects you can rearrange the inlets on the top level of the patch to something that is easier to follow: i.e 1 being the filter type select, 2 and 3 being the audio inlets etc:

/encap04.png

If you follow the cords from 2 and 3, they are now feeding the **svf~** objects. If you close the sub-patch, you should see that the inputs are now more sensible:

/encap05.png

If you pull up the **Inspector** or the inlets inside the sub-patch you can enter a name into the Comment parameter which will then be displayed when you mouse over the inlet of the sub-patch object. Again, this is a good idea for later use of the patch and/or if you are sending the device or sub-patch to someone else.

Remember, that if an object is to be seen in the UI, it cannot be inside a sub-patch. Also, it is not uncommon to see sub-patches within sub-patches within sub-patches so it is important to either keep the **p** at the start so that you know it’s a sub-patch or adopt a naming convention to stick to for recognition.

Save your patch again and take a copy.

**END OF LAB**
