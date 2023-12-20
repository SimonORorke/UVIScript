# UVIScript Lua scripts for UVI Falcon

The "hybrid" synthesizer / sampler [UVI Falcon]( https://www.uvi.net/falcon) allows its its presets ("programs" in Falcon terminology) to call scripts written in [UVIScript](https://www.uvi.net/uviscript/), a domain-specific scripting language built on top of the [Lua](http://www.lua.org/about.html) scripting language. So far, this repository contains just one script (the only one I have written), together with a Falcon program (preset) that demonstrates the script's use. In this README, I shall share some of what I have learned.

## Control a DAHDSR with macros (no script image GUI)

There are no examples in UVI-supplied sound banks of programs where macros on Falcons standard Info Page control a DAHDSR's attack, decay, sustain and release. Instead, attack, decay, sustain and release knobs are provided on image GUIs defined in scripts. The problem is that Falcon does not provide a way to scale macro values (range 0 to 1) up to required parameter value maxima greater than one. The maxima for the DAHDSR's AttackTime, DecayTime and ReleaseTime parameters are 10, 30 and 20 seconds respectively.

The **DAHDSR Controller** script controls a program-level DAHDSR's attack, decay, sustain and release parameters with macros defined in the standard GUI, avoiding the need for a script-defined image GUI. *(Tip: To develop a script-defined image GUI instead, have a look at  [estevancarlos's tutorial]( https://github.com/estevancarlos/uvi-falcon-scripts).)* **Scripts\DAHDSR Controller.lua** itself is just a stub that references the **Scripts\DahdsrController\DahdsrController.lua** script. The purpose of the stub is to prevent the whole script from becoming embedded in the Falcon program file. 

This is the GUI of **Tibetan Horns**, the example Falcon program that contains the **DAHDSR Controller** script processor:
<img src="Images\Tibetan Horns.png" alt="Tibetan Horns" style="zoom: 80%;" />

To allow the macros to control the DAHDSR's parameters, there are ADSR knobs, as intermediaries, on the script processor itself (shown on Falcon's Events page):
<img src="Images\DAHDSR Controller.png" alt="DAHDSR Controller" style="zoom: 80%;" />

These script processor knobs need to be configured to be modulated by the corresponding macros, like this:
<img src="Images\Assign Macro to Knob.png" alt="Assign Macro to Knob" style="zoom: 80%;" />

A limitation of this technique is that, in order to initialise the values of the Info Page macros and the script processor knobs to the corresponding DAHDSR parameter values, the internal macro **Names** in the program have to be hard-coded into the script. So **DahdsrController.lua** currently contains the **Names** of the ADSR macros in **Tibetan Horns**:
| DisplayName | Name    |
| ----------- | ------- |
| Attack      | Macro 5 |
| Decay       | Macro 6 |
| Sustain     | Macro 7 |
| Release     | Macro 8 |

To avoid having to hard-code the macro **Names** in the script, I have investigated accessing the macros via their **DisplayNames**, which are what are shown in the GUI. Unfortunately, that appears to be impossible.  So, to find the program's internal macro **Names**, look in the Falcon program file (**Programs\Tibetan Horns.uvip** in the example) with a text editor. *Tip: If you save the program file with an XML header in a smart editor, you will see the program's XML colour-coded. This works with Notepad++ (Windows), Visual Studio Code (Windows and Mac) and, I expect, BBEdit/TextWrangler (Mac).* In Falcon program files, a macro is called a **ConstantModulation**.  So look for something like this:

<img src="Images\XML Macro Example.png" alt="XML Macro Example" style="zoom: 80%;" />

To make control of the DAHDSR's parameters by the macros more ergonomic, the script processor also has knobs to specify the maximum number of seconds that may be specified by the Attack, Decay and Release macros and knobs:
<img src="Images\Max Seconds.png" alt="Max Seconds" style="zoom: 80%;" />

For example, anything up to the DAHDSR's ReleaseTime parameter maximum of 20 seconds may be specified as the maximum for the Release macro and knob. The **Tibetan Horns** example program does not provide macros corresponding to the three maximum seconds knobs.  But those could easily be added.

### The Tibetan Horns example program

The **Tibetan Horns** Falcon program is provided primarily as an example host for the **DAHDSR Controller** script processor.  However, its other features may be of interest. I developed the **DAHDSR Controller** script as part of making my own versions of the programs in Falcon's [Organic Pads]( https://www.uvi.net/organic-pads)  sound bank. *The Tibetan Horns example program cannot be played in Falcon unless the Organic Pads sound bank is installed.*

Organic Pads programs have four timbre layers that can be mixed together: Synthesis, Sample, Noise and Texture. In the original programs. the layers are mixed by a single X-Y control on the script-defined GUI. That limits the possible mixing permutations. As my hardware MIDI controllers include five expression pedals, I have replaced the X-Y control with four macros. Each of these macros modulates the gain of a specific layer, and each macro is in turn modulated by the MIDI CC of an expression pedal.  That way I can vary the gains of the four layers completely independently of each other.

The other change in my version of the program compared with the original is that all delay and reverb effects have been bypassed.

### A problem still to solve

Reducing the attack time to zero while playing intermittently causes low volume and then silence. But I have not found the problem to consistently occur with any specific program. When the problem happens, sometimes Falcon then becomes silent altogether, even when switching to a different program. If Falcon is then restarted, sound comes back.