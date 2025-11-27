[日本語の説明はこちら](README_JA.MD)

## What is this?
A gen code of custom ADSR envelope.
The concept is a hybrid one of `adsr~` and `live.adsr~`, and use with `live.adsrui`.

## Requirement
Max 8.3.0 or above. 8.2.x is okay but not in timing accuracy and may cause click noise when retrigger.

## Usage
Create `gen~` object and load the gendsp file.
Input note on/off amplitude, pulse signal for retrigger, and messages from `live.adsrui` to `gen~`. 

![custom-adsr-image](retriggerable-adsr-image.png)

See the sample patch for details.

## Input messages
- loopmode [int]: Switches envelope mode as the same as `live.adsr~`.<br/> 
  0: normal ADSR (default)<br/>
  1: trigger mode (play to end mode in `adsr~`)<br/>
  2: loop mode

- legato [int]: Switches the behavior when received note on while the envelope is active, as the same as `adsr~`.<br/>
  0: retrigger mode (default)<br/>
  none 0: legato mode<br/>

- retrigger [float]: retrigger delay in msec, as the same as `adsr~`.

- speed [float]: speed ratio of the envelope for 'Time\<Vel' feature. (default=1.0)

- float / signal<br/>
  - In the left inlet: note on / note off amplitude in 0 to 1.<br/>
  - In the second inlet: trigger input for retrigger behavior. Must be pulse signal from `click~`.
  - In the third inlet: attack time (the rest of inlets are same as `live.adsr~`)
  - In the fourth inlet: decay time
  - In the fifth inlet: sustain
  - In the sixth inlet: release time
  - In the seventh inlet: initial
  - In the eighth inlet: peak
  - In the ninth inlet: final
  - In the tenth inlet: attack slope
  - In the eleventh inlet: decay slope
  - In the twelfth inlet: release slope

## Output
- in the left outlet: the envelope signal.
- in the second outlet: outputs 1 when the envelope is active. As the same as the second outlet of `live.adsr~`.<br/>
  \<TIP\> you can make `mute` message for `thispoly~` from this output and `edge~`.<br/>
  Also, you can use this signal for `mc.noteallocator~`. (see `mc` tab in the sample patch)
- in the third outlet: outputs 1 when the envelope is active except release phase. Otherwise outputs 0. As the same as the second outlet of `adsr~`.

## Miscellaneous
- Curve shapes are not perfectly the same as `live.adsr~`.
