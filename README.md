# UVIScript Lua scripts for UVI Falcon

The "hybrid" synthesizer / sampler [UVI Falcon]( https://www.uvi.net/falcon) allows its its presets ("programs" in Falcon terminology) to call scripts written in [UVIScript](https://www.uvi.net/uviscript/), a domain-specific scripting language built on top of the [Lua](http://www.lua.org/about.html) scripting language. So far, this repository contains just one script (the only one I have written), together with a Falcon program (preset) that demonstrates the script's use. In this README, I shall share some of what I have learned.

## Control a DAHDSR with macros (no script image GUI)

There are no examples in UVI-supplied sound banks of programs where macros on Falcons standard Info Page control a DAHDSR's attack, decay, sustain and release. Instead, attack, decay, sustain and release knobs are provided on image GUIs defined in scripts. The problem is that Falcon does not provide a way to scale macro values (range 0 to 1) up to required parameter value maxima greater than one. For example, the maximum for for the DAHDSR's ReleaseTime parameter is 20 seconds.

The **DAHDSR Controller** script controls a program-level DAHDSR's attack, decay, sustain and release parameters with macros defined in the standard GUI, avoiding the need for a script-defined image GUI.<img src="Images\Tibetan Horns.png" alt="Tibetan Horns" style="zoom: 80%;" />
