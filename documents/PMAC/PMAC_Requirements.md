EPICS PMAC Module Requirements
==============================

Introduction
------------

This document is the starting point for a set of requirements 
for an EPICS asyn type 3 driver for the Delta Tau PMAC motion
controller.  It contains a bullet point list of requirements and 
an EPICS record interface table.

### Requirements list

* The driver will be based on an Asyn model 3 driver.
* Each PMAC axis will be made available to the motor record interface.
* The driver will support PMAC coordinated axes.
* Each PMAC coordinate system degree of freedom will be made available to the motor record interface.
* The driver will be able to switch axes into and out of coordinate systems.
* The driver will be able to write a PLC or motion program to the PMAC controller.
* The driver will be able to fill a buffer of positions for use by the PMAC controller.
* The driver will support the model 3 trajectory interface.
* The driver will be able to start, stop, pause and resume complex motion described by motion programs.
* The driver will initiate and provide feedback of complex homing routines.
* The driver will support deferred moves and deferred coordinated moves for multiple axes.
* The driver will support setting and releasing of brakes.
* The driver will support detection of encoder loss/fault.
* The driver will support initiation of position compare.
* (Do we add a requirement relating to the locking mechanism here?)

### Interface Control

It is always useful to know which EPICS records within a module are intended for external interaction ("Public"
records) and which records are considered "private" for use by the module itself.  Here is a placeholder
for an ICD which can be filled with the list of public records

| Record  | Type | Description |
|---------|------|-------------|
| Example | AI   | Desc        |


