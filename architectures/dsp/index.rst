.. _architecture-high-level:

High Level DSP Architecture
###########################

The Xtensa DSP architecture in UP and SMP modes is supported by |SOF| upstream
today.

The diagram below shows the high-level firmware architecture with the
Baytrail platform integration as an example. The firmware is divided into four
main sections:

#. **Generic microkernel.** The microkernel manages and abstracts the
   DSP hardware for the rest of the system. It also exports C APIs for
   memory allocation, scheduling work, event notifications, and power
   management.

#. **Audio components.** The audio components can be used to form an
   audio processing pipeline from the host DMA buffer to the DSP digital
   audio interface. Audio components will have a source and sink buffer
   where they will usually transform or route audio data as part of their
   processing.

#. **Audio task.** The audio task manages the audio pipelines at run
   time; it manages the transportation of data from source to sink
   component within the pipeline. The pipelines are currently statically
   defined in the firmware, but infrastructure is now in place to allow the
   dynamic creation of pipelines from Linux userspace.

#. **Platform drivers.** The platform drivers are used to control any
   external IP to the DSP IP. This will usually be things like DMA engines
   or DAI (Digital Audio Interface) controllers. These drivers are used by
   the audio components and pipelines to send/receive data to/from the host
   and external codecs.

   ..	figure::  ../images/fw-arch-diag.png
	:align: center
	:alt: SOF Architecture
	:width: 800px

	`Sound Open Firmware Architecture`


Adding a new DSP architecture to SOF
====================================

There are two ways to add support for new DSP architectures to SOF

#. Write a new Hardware Astraction Layer (HAL) for your DSP.

#. Use an existing RTOS that supports your DSP architecture as a HAL for SOF.

Both methods require a working compiler for the new DSP architecture and
preferrably an emulation environment for bringup and debug.

Method 1 - New HAL
------------------

The main work in adding the new architecture HAL is duplicating and porting the 
src/arch directory to your new architecture. The code in the architecture
directory mainly deals with architecture abstraction and initialization of any
architecture IP like MMU, IRQs and caches alongside providing optimized
versions of some common C functions (memcpy, memset, etc) for that architecture.
Adding a new architecture also usually means adding a new host platform too.

Method 2 - Use existing RTOS
----------------------------

This method involves creating a HAL by wrapping the RTOS functions used by SOF
as thinly as possible (i.e. to compile out). It also means removing unused code
from the SOF build in order to use the RTOS version if desireable i.e.
allocator, schedulers, messaging etc. The final stage is to link the SOF audio
code to the RTOS.

Low Level Vendor Specific Architecture
======================================

Intel
-----
.. toctree::
   :maxdepth: 1

   smp/index
