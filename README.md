[![Build Status](https://api.travis-ci.org/libretro/vice-libretro.svg?branch=master)](https://travis-ci.org/github/libretro/vice-libretro)

# VICE LIBRETRO

Port of VICE, the Versatile Commodore Emulator 3.3

Source base: [https://sourceforge.net/projects/vice-emu/files/releases/vice-3.3.tar.gz](https://sourceforge.net/projects/vice-emu/files/releases/vice-3.3.tar.gz)

Supported platforms: Linux, Windows, Apple, Android, emscripten, Switch, Vita

## Default controls

| RetroPad button | Action                  |
|-----------------|-------------------------|
| D-Pad           | Joystick                |
| Left Analog     | Mouse/paddles           |
| B               | Fire button             |
| X               | Space                   |
| L2              | Escape (RUN/STOP)       |
| R2              | Enter (RETURN)          |
| Select          | Toggle virtual keyboard |

| Keyboard key    | Action                  |
|-----------------|-------------------------|
| F12             | Toggle statusbar        |
| RControl        | Switch between joyports |
| End             | Reset                   |

### Virtual keyboard controls

| Button          | Action                  |
|-----------------|-------------------------|
| B / Enter       | Keypress                |
| A               | Toggle transparency     |
| Y               | Toggle ShiftLock        |
| X               | Press Space             |
| Start           | Press Return            |

Long press for sticky keys. Stickying the third key will replace the second.

## Joyport control

Older C64 games tend to use joystick port 1 and newer ones tend to use port 2 for player 1. There are several ways to switch ports in this core:

- Use the core option: `Quick Menu -> Options -> RetroPad Port`
- Bring up the virtual keyboard with `Select` button, and press the key labeled `JOY`
- Press the default keyboard shortcut `Right Control`
- Assign `Switch Joyport` to any RetroPad button under `Quick Menu -> Options`
- Rename the game, eg. `Bruce_Lee_j1.tap` or `Bruce_Lee_(j1).tap` for port 1, and similarly `Bruce_Lee_j2.tap` or `Bruce_Lee_(j2).tap` for port 2
- Add `-j1` or `-j2` parameters in custom command line `.cmd`

## M3U support and disk control

When you have a multi disk game, you can use a M3U playlist file to be able to change disks via RetroArch "Disc Control" interface.

A M3U file is a simple text file with one disk per line ([Wikipedia](https://en.wikipedia.org/wiki/M3U)).

Example:

`Ultima VI - The False Prophet (1990)(Origin Systems).m3u`
```
Ultima VI - The False Prophet (1990)(Origin Systems)(Disk 1 of 3 Side A)(Game).d64
Ultima VI - The False Prophet (1990)(Origin Systems)(Disk 1 of 3 Side B)(Surface).d64
Ultima VI - The False Prophet (1990)(Origin Systems)(Disk 2 of 3 Side A)(Dungeon).d64
Ultima VI - The False Prophet (1990)(Origin Systems)(Disk 2 of 3 Side B)(Populace A).d64
Ultima VI - The False Prophet (1990)(Origin Systems)(Disk 3 of 3 Side A)(Populace B).d64
Ultima VI - The False Prophet (1990)(Origin Systems)(Disk 3 of 3 Side B)(Populace C).d64
```

Path can be absolute or relative to the location of the M3U file.

When the game asks for it, you can change the current disk in the RetroArch "Disc Control" menu:

- Eject the current disk with "Eject Disc"
- Select the right disk index with "Current Disc Index"
- Insert the new disk with "Insert Disc"

By default, RetroArch will display the filename (without extension) of each M3U entry when selecting a disk via the `Current Disc Index` drop-down menu. Custom display labels may be set for each disk using the syntax: `DISK_FILE|DISK_LABEL`. For example, the following M3U file:

`Ultima VI - The False Prophet (1990)(Origin Systems).m3u`
```
Ultima VI - The False Prophet (1990)(Origin Systems)(Disk 1 of 3 Side A)(Game).d64|Game
Ultima VI - The False Prophet (1990)(Origin Systems)(Disk 1 of 3 Side B)(Surface).d64|Surface
Ultima VI - The False Prophet (1990)(Origin Systems)(Disk 2 of 3 Side A)(Dungeon).d64|Dungeon
Ultima VI - The False Prophet (1990)(Origin Systems)(Disk 2 of 3 Side B)(Populace A).d64|Populace A
Ultima VI - The False Prophet (1990)(Origin Systems)(Disk 3 of 3 Side A)(Populace B).d64|Populace B
Ultima VI - The False Prophet (1990)(Origin Systems)(Disk 3 of 3 Side B)(Populace C).d64|Populace C
```

...will be shown in RetroArch's disk selection menu as:

```
1: Game
2: Surface
3: Dungeon
4: Populace A
5: Populace B
6: Populace C
```

If `DISK_LABEL` is intentionally left blank (i.e. `DISK_FILE|`) then only the disk index will be displayed.

For games that require a dedicated save disk, one may be generated automatically by entering the following line in an M3U file: `#SAVEDISK:VolumeName`. `VolumeName` is optional and may be omitted. For example, this will create a blank, unlabelled disk image at disk index 5:

`Elite (1985)(Firebird Software).m3u`
```
Elite (1985)(Firebird Software).d64
#SAVEDISK:
```

Although one save disk is normally sufficient, an arbitrary number of `#SAVEDISK:VolumeName` lines may be included. Save disks are located in the frontend's save directory, with the following name: `[M3U_FILE_NAME].save[DISK_INDEX].d64`.

Save disks generated by the `#SAVEDISK:` keyword are automatically assigned the label: `Save Disk [SAVE_DISK_INDEX]`.

### Extra features

- `#COMMAND:<commands>`
    - Pass arguments to VICE (place first, core name can be skipped)
- `#SAVEDISK:<label>`
    - Create a save disk in `saves`
- `#LABEL:<label>`
    - Alternative label for the next entry
- `<disk>.d64:<prg>`
    - Load a program instead of `"*"`
- `<disk>.d64|<label>`
    - Set a friendly name (shown in "Disc Control")
- `<disks>.zip#<disk>.d64`
    - Specify a disk inside a ZIP with multiple disks (not needed with single file ZIPs)

M3U playlist supports disks, tapes, cartridges and programs.

## ZIP support

ZIPs are extracted to a temporary directory in `saves`, bypassing the default frontend extraction.
The temporary directory is emptied but not removed on exit. ZIP is not repacked, which means saves and highscores are lost.

This allows:

- Automatic M3U playlist generation of all files
- The use of zipped images in M3Us

## JiffyDOS support

External ROM files required in `system/vice`:

| Filename                         | Size   | MD5                              |
|----------------------------------|-------:|----------------------------------|
| **JiffyDOS_C64.bin**             |  8 192 | be09394f0576cf81fa8bacf634daf9a2 |
| **JiffyDOS_C128.bin**            | 16 384 | cbbd1bbcb5e4fd8046b6030ab71fc021 |
| **JiffyDOS_1541-II.bin**         | 16 384 | 1b1e985ea5325a1f46eb7fd9681707bf |
| **JiffyDOS_1571_repl310654.bin** | 32 768 | 41c6cc528e9515ffd0ed9b180f8467c0 |
| **JiffyDOS_1581.bin**            | 32 768 | 20b6885c6dc2d42c38754a365b043d71 |

## Command file operation

VICE command line options are supported by placing the desired command line in a text file with `.cmd` file extension. The command line format is as documented in the [VICE manual](http://vice-emu.sourceforge.net/vice_6.html).

Using this you can overcome limitations of the GUI and set advanced configurations required for running problematic files.


VIC-20 Mega-Cart can be launched via `.cmd`:
```
xvic -cartmega "/path/to/rom/mega-cart-name.rom"
```
**VIC-20 Mega-Cart is supported automatically with NVRAM file directed to `saves`.**


VIC-20 memory expansion can be set via `.cmd`: `-memory (3k/8k/16k/24k/all) / (0/1/2/3/5) / (04/20/40/60/a0)`:
```
xvic -memory 1 "/path/to/rom/some-8k-game.d64"
xvic -memory 8k "/path/to/rom/some-8k-game.d64"
xvic -memory 20 "/path/to/rom/some-8k-game.d64"
```

**VIC-20 memory expansion can be set with filename tags or directory matching:**

- `some game (8k).prg` or `some game [8k].prg` or `/8k/some game.prg`
- `vicdoom (35k).d64`

## Latest features

- Automatic VIC-20 Mega-Cart support (with NVRAM)
- Automatic NIB->G64 conversion
- Region (PAL/NTSC) filepath tags for C64 & VIC-20
- Memory expansion filepath tags for VIC-20
- JiffyDOS support
- Paddles & mouse
- Zoom mode
- Savestates
- Keyrah joystick maps
- Drive Sound Emulation (1541 & 1571 only)
- Reset Type (Autostart, Soft, Hard, Freeze)

## Build instructions

Remember to run `make clean EMUTYPE=x` when building different EMUTYPEs.

Currently working EMUTYPEs:

- `x64`
- `x64sc`
- `x128`
- `xcbm2`
- `xcbm5x0`
- `xpet`
- `xplus4`
- `xscpu64`
- `xvic`

### Linux

```
CC=gcc make -j4
```

### Windows

```
make -j4
```

### Win64 (crossbuild)

```
make platform=wincross64 -j4
```

### Android standalone toolchain Build

```
export path to your standalone toolchain like

export PATH=$PATH:/opt/standtc/bin

make platform=androidstc -j4
```

### Android ndk Build

```
cd jni
ndk-build
```

## Original readme

 ----------------------------------------------------------------------------

  VICE  3.3                                                        Dec 2018


                 _______________
                |      ||      ||
                |      ||      ||____________________________
                |      ||      ||    ||         ||          ||
                |      ||      ||    ||    |    ||    ______||
                |      ||      ||    ||    |----||          ||
                |      ||      ||    ||    |    ||    ------||
                |              ||____||_________||__________||
                 \            // ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                  \          //
                   \________//   Versatile Commodore 8-bit Emulator



 ----------------------------------------------------------------------------

 This release stabilizes and introduces the GTK3 UI as the primary user 
 interface for all major ports. For "lesser" platforms we still have the SDL 
 based interface. All other user interfaces have been removed. RIP

 We are still looking for dedicated maintainers for the Windows- and macOS 
 ports, if you want to help improving these and/or want to provide binaries, 
 please get in touch.

 ----------------------------------------------------------------------------

 This is version 3.3 of VICE, the multi-platform C64, C128, VIC20,
 PET, PLUS4 and CBM-II emulator.  This version can be compiled for
 MSDOS, Win32, OS/2, BeOS, QNX 4.x, QNX 6.x, AmigaOS, Dingoo,
 Syllable, SkyOS and for most Unix systems provided with
 the X Window System version 11, R5 or later.

 The following programs are included:

   - x64, a C64 emulator;

   - x64sc, a C64 emulator focused on accuracy;

   - xscpu64, a C64 emulator with a SuperCPU cart;

   - x64dtv, a C64 Direct-to-TV (DTV) emulator;

   - x128, a C128 emulator;

   - xvic, a VIC20 emulator;

   - xpet, a PET emulator;

   - xplus4, a PLUS4 emulator;

   - xcbm2, a CBM-6x0/7x0 emulator;

   - xcbm5x0, a CBM-5x0 emulator;

   - vsid, a SID player;

   - c1541, a stand-alone disk image maintenance utility;

   - petcat, a CBM BASIC de-tokenizer;

   - cartconv, a C64/C128 cartridge conversion program.

 Moreover, the following documents are provided in both source and
 binary distributions:

   - README, this file;

   - COPYING, the GNU General Public License under which VICE is
     distributed -- *please read it before using the program*;

   - INSTALL, installation instructions;

   - NEWS, list of user-visible changes between this and older versions of VICE;

   - FEEDBACK, something you should read before reporting bugs or sending mail 
     to any of the authors;

 The following documents are provided in the source distribution:

   - in the doc directory:

     - vice.texi, the complete documentation; documentation in various different
       formats may be generated from it (vice.txt, vice.pdf, vice.chm, vice.hlp, 
       vice.guide, vice.inf, vice.info)
     - iec-bus.txt, overview of the VICE IEC bus emulation;
     - CIA-README.txt, overview of the VICE CIA emulation;
     - coding-guidelines.txt, description of VICE coding style
     - Documentation-Howto.txt, information on how to edit the documentation
     - Doxygen-Howto.txt, how to generate doxygen documentation
     - Release-Howto.txt, some hints and reminders on how to create a release
       tarball (for maintainers)
     - ffmpeg-support.txt, this document details the results of test compiles 
       with regards to the included ffmpeg library.
     - SDL-support.txt, this document details the results of test compiles with 
       regards to the SDL 1 and SDL 2 support.
     - hardware-sids.txt, this document details the hardware SIDs and how they 
       are supported on the various platforms.

   - in the doc/readmes directory:

     - Readme-GTK3.txt, GTK3 specific documentation;
     - Readme-SDL.txt, SDL specific documentation;
     - Readme-SDL2.txt, SDL2 specific documentation;

   - in the doc/building directory:

     - Windows-MinGW-GTK3-Howto.txt, how to compile the Windows port using MingGW/MSYS2
     - Linux-GTK3-Howto.txt, how to compile the GTK3 port on Linux
     - GTK3-macOS.txt, how to compile the Mac OSX port with GTK3 UI
     - SDL-Howto.txt, how to compile the SDL port on various platforms
     - Android-Howto, how to compile the Android SDL port
     - FreeBSD-GTK3-Howto.txt, hints on compiling the GTK3 port on FreeBSD
     - NetBSD-howto.txt, hints on compiling for NetBSD

 For the latest news, have a look at the VICE home page:

     http://vice-emu.sourceforge.net/

 Also some information that was previously included in the distribution has been
 moved into our wiki at sourceforge:

   - The TODO list,
     http://vice-emu.sourceforge.net/wiki/index.php/Todo#New_Features

   - The list of known BUGS,
     http://vice-emu.sourceforge.net/wiki/index.php/Todo#Known_Issues

 New versions are made available quite often, so please stay tuned.


 Have fun!


 Copyright notice
 ----------------

 VICE, the Versatile Commodore Emulator

    Core Team Members:
    2005-2018 Marco van den Heuvel
    2007-2018 Fabrizio Gennari
    2009-2018 Groepaz
    2010-2018 Olaf Seibert
    2011-2018 Marcus Sutton
    2011-2018 Kajtar Zsolt
    2016-2018 AreaScout
    2016-2018 Bas Wassink
    2017-2018 Michael C. Martin

    Inactive/Ex Team Members:
    1999-2017 Martin Pottendorfer
    2011-2016 Stefan Haubenthal
    2015-2016 BSzili
    1999-2016 Andreas Matthies
    2007-2015 Daniel Kahlin
    2009-2015 Errol Smith
    2012-2014 Benjamin 'BeRo' Rosseaux
    2011-2014 Ulrich Schulz
    2011-2014 Thomas Giesel
    2009-2014 Ingo Korb
    2008-2014 Antti S. Lankila
    2006-2014 Christian Vogelgsang
    1998-2014 Dag Lem
    2000-2011 Spiro Trikaliotis
    2007-2011 Hannu Nuotio
    1998-2010 Andreas Boose
    1998-2010 Tibor Biczo
    2007-2010 M. Kiesel
    1999-2007 Andreas Dehmel
    2003-2005 David Hansel
    2000-2004 Markus Brenner
    1999-2004 Thomas Bretz
    1997-2001 Daniel Sladic
    1996-2001 André Fachat
    1996-1999 Ettore Perazzoli
    1993-1994, 1997-1999 Teemu Rantanen
    1993-1996 Jouko Valta
    1993-1994 Jarkko Sonninen

    Translation Team Members:
    2009-2017 Mikkel Holm Olsen
    2000-2017 Martin Pottendorfer
    2011-2017 Manuel Antonio Rodriguez Bas
    2004-2017 Paul Dubé
    2006-2017 Czirkos Zoltan
    2006-2017 Karai Csaba
    2001-2017 Andrea Musuruane
    2011-2016 Jesse Lee
    2005-2017 Marco van den Heuvel
    2011-2017 Jarek Sobolewski
    2010-2017 Michael Litvinov
    2000-2017 Peter Krefting
    2008-2017 Emir Akaydin

  This program is free software; you can redistribute it and/or
  modify it under the terms of the GNU General Public License as
  published by the Free Software Foundation; either version 2 of the
  License, or (at your option) any later version.

  This program is distributed in the hope that it will be useful,
  but WITHOUT ANY WARRANTY; without even the implied warranty of
  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
  GNU General Public License for more details.

  You should have received a copy of the GNU General Public License
  along with this program; if not, write to the Free Software
  Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA
  02111-1307  USA

 The ROM files in the `C128`, `C64`, `CBM-II`, `DRIVES`, `PET`, `PLUS4`
 `PRINTER` and `VIC20` directories are Copyright C by Commodore
 Business Machines.

 The ROM files in the `C64DTV` directory are Copyright C by Commodore
 Business Machines, as well as Mammoth Toys, a division of nsi ltd.,
 Digital Concepts DC studios inc., Ironstone Partners ltd., and
 Toy:Lobster company ltd.

### NIBTOOLS

(C) 2005 Pete Rittwage and the C64 Preservation Project team

It is based on MNIB which is copyrighted
(C) 2000 Markus Brenner

