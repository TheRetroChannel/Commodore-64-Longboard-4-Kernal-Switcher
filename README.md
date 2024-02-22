# Commodore 64 Longboard 4 Kernal Switcher

![palpcb](https://github.com/TheRetroChannel/Commodore-64-Longboard-4-Kernal-Switcher/blob/main/images/PALPCB.png)

## Introduction

This is a drop in Kernal ROM replacement for the Commodore 64 Longboard variants (ASSY 326298*, KU-14194HB, 250407, 250425 and 250466). It can hold 4 Kernals that are selectable using the RESOTRE key on the C64. It is available for purchase on my [Tindie store](https://www.tindie.com/products/theretrochannel/commdore-64-4-kernal-switchless-rom-with-jiffydos/) or you have the option to build your own.

ASSY 326298 requires a fix to the reset signal. You can find details on how to do it and a few other fixes for this board revision [here](https://youtu.be/agDFLPP9yIw)

You can find a build video [here](https://youtu.be/4KRom9XnFa4)

And installation video [here](https://youtu.be/RACQwrXHKg0)

This is based on the [C64 Kernal Switcher](https://github.com/tebl/C64-Kernal-Switcher) by tebl. Which in turn is based on the SKS64 switchless Kernal switcher by bwack.

---
Designed by BWACK. CERN OPEN HARDWARE LICENSE v1.2 http://www.ohwr.org/

The CAD files are available on GitHub: https://github.com/bwack/C64-Switchless-Multi-Kernal-27C256-adapter

All text above must be included in any redistribution**

---

The version here was made in an effort to shrink everything down to the physical size of the original 2364 mask ROM and place everything on the underside, similar to my other replacement ROMs available on [Tindie](https://www.tindie.com/products/theretrochannel/commodore-64-and-1541-replacement-roms/). This results in a very neat looking package leaving only the silkscreen text on the topside.

## Parts
In order to build one of these the following is required:

* PCB - [NTSC](https://www.pcbway.com/project/shareproject/Commdore_64_Longboard_4_Kernal_Switchless_ROM_NTSC_version_9adf7ceb.html) or [PAL](https://www.pcbway.com/project/shareproject/Commdore_64_Longboard_4_Kernal_Switchless_ROM_PAL_version_bd61956a.html) (see notes below)
* 28C256 EEPROM (SOIC-28)
* PIC12F629 or PIC12F675 (SOIC-8)
* 390ohm Resistor (0603 SMD)
* 0.1uF Capacitor (0603 SMD)
* 2.54mm Right angle pin headers x5
* 2.54mm Male round machine pin headers x24
* A few random bits of wire
* JiffyDOS license (also required to build JaffyDOS)

Also required is a programmer such as the XGecu TL-866/TL-866ii or T48/T56 and adaptors for SOIC-8 and SOIC-28 packages.

The silkscreen on the PCB lists the intended ROMs to be used with this design. It is also possible to change the silkscreen by obtaining the [design files](https://github.com/TheRetroChannel/Commodore-64-Longboard-4-Kernal-Switcher/tree/main/design%20files), however making multiple PCBs, designs and instructions based on every combination of Kernal ROMs is not something I intend on doing. The reason I chose these particular Kernals and their usage will be discussed later.

Note that there are two designs available, the reason for this is because the EXOS Kernal is unique for PAL and NTSC systems, all other Kernals will work with both PAL and NTSC. The easiest way to order [NTSC](https://www.pcbway.com/project/shareproject/Commdore_64_Longboard_4_Kernal_Switchless_ROM_NTSC_version_9adf7ceb.html) or [PAL](https://www.pcbway.com/project/shareproject/Commdore_64_Longboard_4_Kernal_Switchless_ROM_PAL_version_bd61956a.html) PCBs is through the shared project page on PCBWay. Add to your cart and then scroll down to find the Remove product No. option and select Specify a location. PCBWay should then print their product number under the 28C256. You should be safe to leave all other options as the default ie. 2 layer FR-4, 1.6mm board thickness, HASL with lead, etc. Alternatively you can download the gerber files

Note PCBWay give me a small credit when ordering using the shared project page.

## Programming
Prior to soldering the components, both the PIC and EEPROM need to be programmed. Once soldered they cannot be reprogrammed without removing them from the PCB. The following assumes you already have a programmer, SOIC-8 and SOIC-28 adaptors and know how to use it all.

Download and program the hex file for the [PIC12F629](https://github.com/TheRetroChannel/Commodore-64-Longboard-4-Kernal-Switcher/blob/main/PIC%20files/12F629.hex) or [PIC12F675](https://github.com/TheRetroChannel/Commodore-64-Longboard-4-Kernal-Switcher/blob/main/PIC%20files/12F675.hex) - you must use the hex file that matches your IC.

Download the [901227-03 Kernal ROM](https://myoldcomputer.nl/Files/Firmware/Computers/C64%20-%20901227-03%20-%20Commodore%20(DBE3E7C7)%20Kernal.rom), [JaffyDOS patcher](http://blog.worldofjani.com/?p=3544) and [EXOS V3 ROM](https://myoldcomputer.nl/Files/Firmware/Computers/C64%20-%20EXOS%20V3%20-%2064er%20(26F3339E)%20(from%20convert%20tool).rom). You will also need to purchase a [JiffyDOS overlay image](https://store.go4retro.com/jiffydos-64-kernal-rom-overlay-image/) as it is still sold under license and every copy of JiffyDOS requires a separate license. Thanks to Jani for JaffyDOS and myoldcomputer.nl for keeping a great archive of [Commodore Kernals](https://myoldcomputer.nl/technical-info/commodore-8-bit-roms/computer-roms/).

Use the instructions on World of Jani to patch JiffyDOS and create JaffyDOS. You can also customise it to your liking.

A small change will need to be made to EXOS if you're planning on using it with an NTSC machine. If the machine is PAL, the following code change is not required.

**For EXOS to work properly with NTSC, one byte needs to be changed at offset 18C0 from EA to C1. You can use a program like HxD to make this change, just be sure you don't change anything else (unless you know what you're doing).**

Finally you should have all 4 ROMs ready to be programmed onto the 28C256. Before doing so, I would recommend testing them with an emulator like VICE to confirm they boot and look correct.

You can either combine the ROMs into one file using the copy /B command in Windows or you may be able to load each one into your programming software one after the other if it allows. Either way you should end up with a 32KB ROM with the ROMs arranged in the same order as the silkscreen on the PCB 1. 901227-03, 2. JiffyDOS, 3. JaffyDOS, 4. EXOS

## Build
This should be a straightforward process if you are already used to working with SMD componenets. I prefer to solder everything in the following order: EEPROM, PIC, capacitor, resistor, right angle headers and finally round pin headers. You may want to stick the round pin headers in a round pin socket before soldering to make sure the alignment stays correct.

## Install
Remove the Kernal ROM at U4 on the C64 and insert this replacement, you may need to desolder the Kernal ROM and install a socket if it is not in one already. Connect a wire from the RSET pin on the Kernal ROM to the RESET line on the C64 mainboard. Likewise connect RSTR to the RESTORE line. Please see the [images](https://github.com/TheRetroChannel/Commodore-64-Longboard-4-Kernal-Switcher/tree/main/images) folder for where to find these signal on each C64 revision

Finally connect the orignal power LED to the pins labelled LED+ LED- LED+, the orientation does not matter as they are the same when flipped. Note some C64s have a shorter LED cable that may not reach over to the Kernal ROM, in this case you will need to extend the wires, an easy way to do this is by using some male to female dupont connectors.

## Usage
When you first power on the system you should notice the power LED flash rapidly for a second, if this does not happen double check your RESET and RESTORE wires are connected to the correct places on the C64 and that you have plugged the power LED into the Kernal ROM and not the original C64 LED connector.

On first boot you should be greeted with the 1st Kernal and everthing should look as normal. Holding the RESTORE key should cause the power LED to flash once every second or so, this is how we control the switcher and releasing the RESTORE key will "confirm" our selection.

* 1 flash: Resets the machine
* 2 flashes: Move to next Kernal
* 3 flashes: Jump to 1st Kernal
* 4 flashes: Jump to 2nd Kernal
* 5 flashes: Jump to 3rd Kernal
* 6 flashes: Jump to 4th Kernal
  
## Why these Kernals?
I chose these because they suit most common situations which I'll cover below. There are many other Kernals out there but a lot of them are more specific in their use cases, of course you are free to use whichever ones you prefer but the silkscreen will not match unless you change it yourself or just put a label over it.

**901227-03**: The final version of the original Commodore 64 Kernal. This is a must have for full compatibilty and cassette support, and it feels like home. This is my go to when using a datasette or in the super rare case I find something that doesn't work with the other Kernals.

**[JiffyDOS](https://www.c64-wiki.com/wiki/JiffyDOS)**: Undoubtedly the most popular Kernal replacement. It offers around a 10X speed boost when paired with JiffyDOS in the 1541 or SD2IEC (which supports Jiffy routines) and pi1541 (requires the 1541 JiffyDOS binary). There is also a huge amount of shortcut keys, some of which provide added functionality over the original C64 Kernal. The trade off is some slight compatibilty issues with certain software and the removal of cassette support, although you'd be hard pressed to find something these days that doesn't work with JiffyDOS. This is my go to when paired with a 1541 with JiffyDOS.

**[JaffyDOS](http://blog.worldofjani.com/?p=3544)**: As the name suggests, it's very similar to JiffyDOS. The biggest difference is the built in file browser. It was originally designed by Jani to work with the SD2IEC but also works the 1541 and pi1541, although full functionality is not guaranteed. Cassette and RS232 support is removed to make way for the extra features. As Jiffy is required to build Jaffy it made sense to just include both. This is my go to when using the SD2IEC and pi1541.

**[EXOS](https://rr.pokefinder.org/wiki/Exos)**: Originally published in the German 64'er magazine by J Schemmel, EXOS is a fastloader that doesn't require any modifications to the disk drive. It offers a speed boost similar to that of JiffyDOS but without the solid compatibilty JiffyDOS offers. While a lot of programs will play nice, expect issues with software that includes its own fastloader routines, or things that rely heavily on the original 1541 timings (many modern demos). It will also have issues if JiffyDOS is enabled in the 1541 drive, and requires a different version for PAL and NTSC (hence why there are two options to choose from). Just like Jiffy and Jaffy it does not offer cassette support. This is my go to when using a stock drive that doesn't have JiffyDOS.

---

To help me continue doing this kind of thing or just to say thanks, consider signing up as a [Patron](https://www.patreon.com/theretrochannel)
