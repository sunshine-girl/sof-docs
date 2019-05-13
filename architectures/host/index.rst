.. _architecture-host:

Host Architecture
#################

SOF Driver Architecture
=======================

The ASoC driver architecture for |SOF| is shown in the diagram below.
The driver architecture is also split into four layers, like a protocol
stack, each with a different purpose.

#. **Machine driver.** The ASoC machine driver does all the
   machine/board audio hardware integration. It also glues the platform
   driver and drivers for any codec(s) together so they appear as a single
   ALSA sound card. |SOF| can reuse existing upstream machine drivers (as
   only the platform name needs to be changed) or can have bespoke machine
   drivers.

#. **Generic PCM Driver.** The PCM driver creates ALSA PCMs, DAPM, and
   kcontrols based on the topology data loaded at run time. The PCM driver
   also allocates buffers for DMA and registers with run time PM. It is
   architecture and platform generic code.

#. **Generic IPC driver.** The IPC driver is the messaging bridge
   between the host and DSP and defines the messaging ABI and protocol. It
   is architecture and platform generic code.

#. **DSP Platform Driver.** The platform driver is a platform specific
   driver that abstracts the low level platform DSP hardware into a common
   generic API that is used by the upper layers. This includes code that
   will initialize the DSP and boot the firmware.


   ..	figure::  ../images/driver-arch-diag.png
	:align: center
	:alt: SOF Driver Architecture
	:width: 800px

	`Sound Open Firmware Driver Architecture`

The right-hand side of the diagram shows the mailbox/doorbell mechanism and the DSP.

The PCM and IPC drivers can be reused without modification on every
platform. The platform differentiation will occur via the topology data
and firmware. There is also scope for differentiation via the machine
driver and platform driver. The ACPI or Device Tree could be used to
specify the HW configuration.

