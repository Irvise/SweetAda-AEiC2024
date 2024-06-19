---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://www.sweetada.org/images/banner.jpg
# some information about your slides, markdown enabled
title: "SweetAda: a Multi-architecture Embedded Development Framework"
info: |
  ## A showcase of SweetAda
  Presentation slides for the SweetAda talk given in AEiC 2024

# apply any unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
---

# SweetAda: a Multi-architecture Embedded Development Framework

### A showcase of the power of Ada, anywhere you may dream

[Gabriele Galeotti](https://www.sweetada.org/) & [Fernando Oleo Blanco](https://irvise.xyz/)

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/gabriele-galeotti/SweetAda" target="_blank" alt="GitHub" title="Open SweetAda's Github page"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
---

# Objectives
SweetAda brings a lot to the table, we will showcase the following topics

- Present SweetAda
  - What it is
  - What it supports
  - How to work with it

- Showcase SweetAda
  - SweetAda on an FPGA
  - SweetAda on...

---
---

# Why SweetAda?
<span></span>

It started from a firmware for a small M68k embedded board...

<v-click>

### An idea... run Ada <span v-mark="{ type: 'underline', color: 'red', at: '2'}">everywhere</span>, when the CPU starts

</v-click>

<br>

<v-click at="3">

### Issues started to appear

- Toolchain? 
- RTS (Run Time System)?
- Build system/framework?

</v-click>

<br>

<v-click at="4">

### Goal

It should be capable of running even on an S/390 mainframe! Emulated at least
</v-click>

<!--
Maybe we could add a photo here of the M68k board to make this slide cooler
-->

---
---

# Soooo... Did SweetAda deliver?
Oh yes, it does deliver

### Currently supported architectures

```
~$ ls SweetAda/cpus
AArch64  ARM  AVR  M68k  MicroBlaze  MIPS  NiosII  OpenRISC  PowerPC  RISC-V  SPARC  SuperH  System390  x86  x86-64
```

<v-click>

### Currently supported targets

```
~$ ls SweetAda/platforms
Altera10M50GHRD     Dreamcast     MemecFX12       PC-x86         QEMU-R2D-PLUS          Spartan3A-EK
Amiga-FS-UAE        FRDM-KL46Z    ML605           PC-x86-64      QEMU-RISC-V            Spartan3E-SK
Android             GEMI          MPC8306-SOM     PK-S5D9        QEMU-STM32VLDISCOVERY  STM32F769I
ArduinoUNO          HiFive1       MPC8306-Switch  QEMU-AArch64   Quadra800              System390
Atlas               IntegratorCP  MPC8315e        QEMU-AVR       RaspberryPi3           Template
DE10-Lite           LEON3         MSP432P401R     QEMU-MIPS      REF405EP               VMIPS
DECstation5000.133  M5235BCC      MVME162-510A    QEMU-OpenRISC  SBC5206                XilinxZynqA9
DigiConnectME       Malta         NEORV32         QEMU-PPC64     SPARCstation5          ZOOM
```
Some even have subplatforms (different implementations)
```
~$ ls SweetAda/platforms/NEORV32
platform-DE10-Lite    platform-GHDL    platform-ULX3S-Litex
```

</v-click>

---
transition: slide-up
---

# A few extra goodies

### Build your own toolchain

```
~$ ls SweetAda/toolchains
Binutils.sh  GCC.sh  GDB.sh  GNATTOOLS.sh  linux-python-config.sh  MANUAL.txt
```

Scripts to build GCC/GNAT for all of these targets, though it is not fully automated

<v-click>

### It's own small C-lib

```
~$ ls SweetAda/clibrary
ada_interface.h  clibrary.gpr      ctype.c         c_wrappers.ads  gnat.adc  stdio.h   string.c   strings.h
assert.c         clibrary.h        ctype.h         errno.c         Makefile  stdlib.c  string.h
assert.h         configuration.in  c_wrappers.adb  errno.h         stdio.c   stdlib.h  strings.c
```

</v-click>

<v-click>

### A few drivers...

```
~$ ls SweetAda/drivers
am7990.adb        etherlinkiii.ads  ide.ads        pc.adb      piix.adb   uart16x50.adb  xps.ads
am7990.ads        ethernet.adb      Makefile       pc.ads      piix.ads   uart16x50.ads  z8530.adb
blockdevices.adb  ethernet.ads      mc146818a.adb  pci.adb     pl011.adb  upd4991a.adb   z8530.ads
blockdevices.ads  goldfish.adb      mc146818a.ads  pci.ads     pl011.ads  upd4991a.ads
configuration.in  goldfish.ads      ne2000.adb     pcican.adb  pl110.adb  vga.adb
etherlinkiii.adb  ide.adb           ne2000.ads     pcican.ads  pl110.ads  vga.ads
```

</v-click>

---
---

# SweetAda's technical stack
How to make all this possible

### GCC/GNAT compiler, the driving force

Needs no introduction nor explanation. GCC with all the architectures it supports, is the central pillar

<v-click>

### Good old `make`

Used as the skeleton for the build. It selects targets, runtimes, toolchains, flags, autogenerates/processes files, prints diagnostics...

</v-click>

<v-click>

### GPRBuild, the Ada heavyweight builder

Runs the build for the final binary/kernel

</v-click>

<v-click>

### And some configuration files

Each platform works a bit differently. A config file is present to help with the build config, post-build and upload. Python, shell and some legacy TCL is also used

</v-click>

---
---

# A tipical workflow, step by step, using SweetAda I

## SweetAda setup
<br>

<v-click>

### First things first, get SweetAda & find help
```
git clone https://github.com/gabriele-galeotti/SweetAda && cd SweetAda
make help
```

</v-click>

<v-click>

### Get a toolchain

- Either provide one / your own (SweetAda is **that** flexible)
- Or build one using script found in `~$ ls SweetAda/toolchains`

</v-click>

<v-click>

## You are ready to rock and roll

</v-click>

---
---

# A tipical workflow, step by step, using SweetAda II

## CPU & Board setup

<v-click>

### Build the RTS (Run Time System)
SweetAda supports both the `zfp` (zero-footprint profile) & `sfp` (small-footprint)
```
make CPU=RISC-V RTS=sfp rts # Or RTS=zfp
# make CPU=RISC-V RTS=sfp TOOLCHAIN_NAME=riscv64-elf rts
```

</v-click>

<v-click>

### Select a board and files for the build process
This will select the boards specific files and configuration for the build
```
make PLATFORM=NEORV32 SUBPLATFORM=ULX3S-Litex createkernelcfg
```

</v-click>

<v-click>

### Configuration of the build (flags, post-process steps, application to be built...)
```
make TOOLCHAIN_NAME=riscv64-elf PLATFORM=NEORV32 SUBPLATFORM=ULX3S-Litex configure
```

</v-click>

---
---

# A tipical workflow, step by step, using SweetAda III

### Modify the default configuration as you see fit
```
[...]
TOOLCHAIN PREFIX:        /opt/toolchains
TOOLCHAIN NAME:          riscv64-elf
[...]
GCC MULTIDIR:            rv32im/ilp32
RTS:                     sfp
GNAT.ADC PROFILE:        sfp
ADA MODE:                ADA22
[...]
USE LIBADA:              Y
USE CLIBRARY:            
OPTIMIZATION LEVEL:      2
ADA GCC SWITCHES (RTS):  -mcmodel=medany
C GCC SWITCHES (RTS):    -mcmodel=medany
GCC SWITCHES (PLATFORM): -march=rv32i2p0_mc -mabi=ilp32
LOWLEVEL FILES:          startup.S llkernel.S
GCC SWITCHES (LOWLEVEL): 
LD SCRIPT:               linker.lds
LD SWITCHES:             -m elf32lriscv --defsym _riscv_mtime_mmap=0xFFFFF400 --defsym _riscv_mtimecmp_mmap=0xFFFFF408
OBJCOPY SWITCHES:        
OBJDUMP SWITCHES:        
```

---
---

# A tipical workflow, step by step, using SweetAda IIII

<v-click>

### Build the kernel, application into a binary
GPRBuild takes over the build
```
make all # or make kernel
make postbuild # run helper steps if needed to get the binary ready
```

</v-click>

<v-click>

### Run the application
Each target has its own way of running SweetAda (launch emulator, upload via serial, JTAG...)
```
make run
# or
make session-start # make session-end
```
<br>

</v-click>

<v-click>

## Enjoy!

</v-click>

---
transition: fade-out
layout: center
---

# DEMOnstration time
SweetAda running on...

- [ULX3S](https://ulx3s.github.io/) FPGA, [NEORV32](https://neorv32.org/) softcore, [Litex](https://github.com/enjoy-digital/litex) build
  - RISC-V 32-bit IMC core, 50 MHz, Timer, UART, Leds
- FRDM-KL46Z ARM board
  - LEDS, GDB-debugging session
- PC-X86, QEMU-ROM
  - VGA, TCP-IP stack

---
transition: fade-out
---

# SweetAda's strengths
SweetAda offers a lot to the Ada (and wider) programming community

<v-clicks>

- 📤 **Portability** - run Ada code on a large number of architectures, boards and emulators!

- 🧑‍💻 **Developer friendly & Hackable** - show expand, tweak, improve and automate things as your needs evolve!
- 🚄 **Fast & easy work** - prepare, build and upload applications with just a few commands!
- 🌕 **Full support** - supports and builds several runtimes, with exceptions, interrupts, and much more!
- 🚀 **Open development** - has had tremendous growth and contributions are more than welcome!
- 📖 **MIT licensed** - hack, develop, integrate it however you like thanks to its liberal license!

</v-clicks>

<v-click>

### Future work?

Improve the RTS, more drivers, quality-of-life improvements, **documentation**, board support...

</v-click>

---
layout: cover
background: ./memec.png
class: text-center
---

# Thank you very much!

<br>

[Gabriele Galeotti](https://www.sweetada.org/) & [Fernando Oleo Blanco](https://irvise.xyz/)

[gabriele.galeotti@sweetada.org](mailto:gabriele.galeotti@sweetada.org)
