.. _introduction:

Introduction to the SOF Project
###############################

|SOF| (SOF) is an open source audio Data Signal Processing (DSP) firmware
infrastructure and SDK. SOF provides infrastructure, real-time control pieces,
and audio drivers as a community project. The project is governed by the |SOF|
|TSC| (TSC) who are prominent and active developers from the community.
SOF is developed in public and hosted on the github platform.

The firmware and SDK are intended for developers who are interested in
audio or signal processing on modern DSPs or who are interested in
microkernels that run on small but powerful processors. SOF provides a
framework where audio developers can create, test and tune :-

#. Audio processing pipelines and topologies.

#. Audio processing components.

#. DSP infrastructure and drivers.

#. Host OS infrastructure and drivers.

|SOF| has a modular and generic codebase and can be ported to different DSP 
architectures or host platforms. See list of currently supported DSP
architecures and supported platforms.

The SOF audio testbench also allows audio processing components to run on a
developers PC for development and testing purposes.


SDK Introduction
================

The |SOF| SDK comprises of the following ingredients:

SOF source code, tools and topologies
-------------------------------------

The firmware, tools and topologies all exist in the main SOF git repository
and at a high level it contains :-

#. Firmware - written in C with some architecture-specific assembler; it does not link to external dependencies.

#. Test Bench - allows firmware components and pipelines to run on developers host PC. 

#. Image Tools - C tools for converting ELF files to binary firmware images that can run on HW.

#. Debug Tools - Scripts and tools that can be used to debug firmware.

#. Trace Tools - Text based tools that can display tracing data from firmware.

#. Tuning Tools - Matlab/Octave scripts that can be used to create tuning coefficients for audio components.

#. Runtime Tools - command line applications that can be used to exchange data with running firmware.

#. Topologies - Real and example topologies showing pipeline construction.


Host OS Drivers
---------------

SOF can be configured and controlled by a host OS driver or can optionally run
as a stand alone firmware. SOF host drivers currently supports Linux OS today. 

The SOF driver has a modular stack based architecture that is dual licensed
BSD & GPL code allowing it to be ported to other OSes and RTOSes. 

The host driver is responsible for :-

#. Loading firmware from host file system into DSP memories and booting.

#. Loading topologies from host file system into DSP.

#. Exposing audio control devices to applications.

#. Exposing audio data endpoints to applications.

#. Managing IPC communication between host and DSP.

#. Abstraction of host side DSP hardware to common API operations.

The Linux SOF ALSA/ASoC driver is upstream in Linux v5.2 onwards.


Firmware Toolchain
------------------

GNU GCC can be used as a free SOF compiler alongside proprietary DSP vendor
compilers. The choice of compiler is up to the user depending on features
and budget. GCC complier is free.
 
 
DSP Emulator
------------

Qemu can be used to provide a functional emulator to simultaneously trace and
debug driver and DSP firmware code. Proprietary emulators are also available.

Emulation is also used within SOF CI for feature validation prior to merging
new code.


General FAQ
===========

What license does the firmware use ?
  The firmware is released using a standard BSD 3 clause license with some
  files released under MIT.

Do I need to open source my firmware code changes ?
  No. The firmware BSD and MIT licensed code means you can keep code
  changes private. Patches are always welcomed if do decide to open source
  work.

What license does the host driver use ?  
  Most of the host driver code is dual licensed BSD or GLPLv2 only 
  (users choice). The part of the driver that is GPLv2 only is the Linux
  integration layer at the top of the driver stack

Do I need to open source my driver code changes ?
  No for the bottom two layers of the driver stack. i.e. if you are porting the 
  driver to another OS, these changes can be kept private. Please note that the
  driver GPL source files are all Linux specific and should not be ported to
  another OS.

How can I get involved ?
  There are several ways to get involved, only the github method is mandatory :-
#. Join github and fork the project.
#. Join the developer mailing list: http://alsa-project.org/mailman/listinfo/sound-open-firmware
#. Join the IRC channel. TODO name.

What is the development model ?
  |SOF| is entirely developed on github. Patches via a Pull Request are
  reviewed, discussed and tested by CI before being merged. The intended
  release cadence will likely be every 6 - 8 weeks. There will be a stable
  release tagged after passing QA then development will continue for the
  next release.

Who is working on |SOF| ?
  Proffesional developers from a number of companies (please check the git
  logs if you want to know) and some hobbyist developers too.

How do I add support for host architecture X ?
  Please see the SOF architecture pages.

How do I add support for host platform X ?
  Adding a new host platform is a lot simpler than adding a new DSP
  architecture. A new host platform consists of adding a new src/platform/
  directory, together with mappings for memory, IRQs, GPIOs and peripheral
  devices in the DSP memory space. New drivers may also have to be added
  (e.g. for DMA, I2S) to the drivers directory.

How do I port to other OSes ?
  There is nothing stopping the firmware working with non Linux based OSes
  providing a driver exists or can be written for that OS. The main area
  for potential optimization in porting to another OS is aligning the IPC
  (Inter processor Communication) mechanism to the audio driver flow for
  that OS. Sound Open Firmware has IPC optimized for the ALSA driver flow,
  but it's easy enough to reuse this IPC on other OSes or add a completely
  new IPC for the OS of your choice. The IPC ABI is defined in
  src/include/uapi/ and the IPC IO logic lives in src/ipc/.

  The current upstream IPC uses a memory mapped doorbell and mailbox to
  pass messages between the host and DSP. Non memory mapped IO (like I2C)
  can also be supported by adding new doorbell and mailbox driver in your
  platform code.

What audio components are supported ?
  |SOF| now supports a small library of free and opne source components that are
  distrubuted alongside the source code. SOF can also support proprietary
  audio processing components providing they are wrapped to use the SOF
  component API.
  Please see the audio components page for a list of the open source components
  and thier capablilities.

How do I create my own pipelines ?
  Pipelines are currently defined using the M4 macro processing language. The M4
  topology is then preprocessed to the alsaconf format before being compiled
  into a binary. The M4 topologies will soon be deprecated in favour an
  Eclipse based GUI for pipeline construction.
  
  Today both static (built in) and dynamic (loaded at runtime) pipelines are
  supported in upstream.

Can I add my own media encoder/decoders ?
  Yes.

Can I add non-audio functions ?
  Yes, the instruction sets used by DSPs are also good at non audio
  processing tasks too. e.g. low power sensor signal processing. Providing
  your DSP has physical IO ports to connect other non audio devices then
  it's possible to process data from these devices too.
