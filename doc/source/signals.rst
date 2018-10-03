.. _signal_indx:

Signals
*******

In EPICS, **Signal** maybe backed by a read-only PV, a single
read-write PV, or a pair of read and write PVs, grouped together.  In
any of those cases, a single value is exposed to `bluesky
<https://nsls-ii.github.io/bluesky>`_.  For more complex hardware, for
example a `motor record
<http://www.aps.anl.gov/bcda/synApps/motor/>`_, the relationships
between the individual process variables needs to be encoded in a
:class:`~device.Device` (a :class:`~epics_motor.EpicsMotor` class
ships with ophyd for this case).  This includes both what **Signals**
are grouped together, but also how to manipulate them a coordinated
fashion to achieve the high-level action (moving a motor, changing a
temperature, opening a valve, or taking data).  More complex devices,
like a diffractometer or a Area Detector, can be assembled out of
simpler component devices.


A ``Signal`` is much like a ``Device`` -- they share almost the same
interface -- but a ``Signal`` has no sub-components. In ophyd's hierarchical,
tree-like representation of a complex piece of hardware, the signals are
the leaves. Each one represents a single PV or a read--write pair of PVs.

.. index:: kind attribute
.. _kind:

:attr:`kind`
-------------

The :attr:`kind` attribute is the means to identify a signal that is 
relevant for handling by a callback.  
:attr:`kind` controls whether the signal's parent
Device will include it in ``read()``, ``read_configuration()``, and/or
``hints.fields``. 
The first use of :attr:`kind` is to inform 
visualization callbacks about the independent and dependent display 
axes for plotting.  
A Component marked as hinted will return a dictionary with that component's fields list.

The :attr:`kind` attribute takes string values of: ``config``, 
``hinted``, ``normal``, and ``omitted``.
These values are like bit flags, a signal could have multiple values.

The value may be set either when the :class:`Signal` is created or
programmatically.
Use the :attr:`kind` attribute when creating a :class:`Signal`
or :class:`Component`, such as:

.. code-block:: python

  from ophyd import Kind

  camera.stats1.total.kind = Kind.hinted
  camera.stats2.total.kind = Kind.hinted

or, as a convenient shortcut (eliminates the import)

.. code-block:: python

  camera.stats1.total.kind = 'hinted'
  camera.stats2.total.kind = 'hinted'

With ophyd v1.2.0 or higher, use :attr:`kind` instead of setting
the :attr:`hints` attribute of the :class:`Device`.  See 
:ref:`hints_fields` for more details.


.. automodule:: ophyd.signal
