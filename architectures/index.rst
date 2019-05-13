.. _architectures:

Supported Architectures
#######################

SOF is intended to run on many different hardware architectures and therefore
it's not tightly or even loosely coupled to any particular DSP or host hardware
architecture. The TSC ensure that any DSP or host architecture specific code
is partitioned to reside in architecture specific directories with generic
APIs to common code.


.. toctree::
   :maxdepth: 2

   host/index
   dsp/index
