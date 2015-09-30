EPICS PMAC Module Requirements
==============================

Introduction
------------

This document is the starting point for a set of requirements 
for an EPICS asyn type 3 driver for the Delta Tau PMAC motion
controller.  It contains a bullet point list of requirements and 
an EPICS record interface table.

### Requirements list

1 General Requirements
1.1 The driver will be based on an Asyn model 3 driver.
   - A type 3 PMAC driver already exists for real axes.
   - The current type 3 driver shall be moved into the PMAC module.
1.2 Each PMAC axis will be made available to the motor record interface.
   - The current type 3 driver already supports individual axes.
2 Coordinate Systems
2.1 The driver will support PMAC coordinated axes.
   - The design shall be taken from the PowerPMAC module.  Implementation will consist of a combination of PowerPMAC and pmacCoord module.
   - Goal is to deprecate pmacCoord, with the functionality migrating over to the PMAC module.
   - Goal is to deprecate pmacUtil, with the functionality migrating over to the PMAC module.
2.2 Each PMAC coordinate system degree of freedom will be made available to the motor record interface.
   - This is implied by implementing pmacCoord as part of the type 3 driver.
2.3 The driver will be able to switch axes into and out of coordinate systems.
   - Giles Knap already has an implementation for this.  The implementation shall be moved into the PMAC module.
3 Complex Motion
3.1 The driver will be able to write a PLC or motion program to the PMAC controller.
   - Requires locking mechanism (described below).  Single buffer to accept arbitrary ASCII data for writing to controller.
3.2 The driver will be able to fill a buffer of positions for use by the PMAC controller.
   - Data buffer on PMAC split into blocks
   - Size of overall buffer defined from startup
   - Size of individual "blocks" defined from startup
   - Writing of blocks will attempt to keep as close behind the PMAC current block pointer as possible
   - PMAC will report its current "block" at a high rate
   - Investigation into writing data directly to memory blocks will take place, therefore bypassing the ASCII comms.
3.3 The driver will support the model 3 trajectory interface.
   - The trajectory interface provides the following driver calls
     - initializeProfile
     - defineProfile
     - buildProfile
     - executeProfile
     - abortProfile
     - readbackProfile
   - These methods can be used to supply a set of PVT data to the PMAC, and to control execution of the motion.
3.4 The driver will be able to start, stop, pause and resume complex motion described by motion programs.
   - Interface needs to allow specification of the motion program number, CS number and an arbitrary set of Q variables.
   - Interface code will be very similar to that already defined for trajectory.
   - Pete Leicester's functioning raster scan program can be used as the initial goal for this interface.
   - The interface between motion program and driver shall be clearly defined, so that additional motion programs can be
    added without needing to investigate the code within the driver.
4 Utilities
4.1 The driver will initiate and provide feedback of complex homing routines.
   - This functionality will be provided by migrating pmacUtil into the PMAC module.
4.2 The driver will support deferred moves and deferred coordinated moves for multiple axes.
   - Giles Knap has an implementation of this already working.  This implementation will be included in the PMAC module.
4.3 The driver will support setting and releasing of brakes.
   - This functionality will be provided by migrating pmacUtil into the PMAC module.
4.4 The driver will support detection of encoder loss/fault.
   - This functionality will be provided by migrating pmacUtil into the PMAC module.
4.5 The driver will support initiation of position compare.
   - This functionality will be provided by migrating pmacUtil into the PMAC module.
5 PMAC Locking
5.1 The drive must offer a locking mechanism for applications that need to download large ASCII (that opens programs, etc).
   - Investigation of results of previous study into the PMAC locking mechanism provided on board.
   - Implementation of a locking program that can sit in front of the PMAC, offering single point locking.
    
### Design

The PMAC module will replace the existing tpmac, pmacUtil and pmacCoord modules and provide a type 3 Asyn driver for the PMAC motion controller family.

![PMAC module high level](https://github.com/dls-controls/epics-mwg-discussions/blob/master/documents/PMAC/PMAC_current.png "PMAC module replaces tpmac, pmacUtil and pmacCoord")

The following steps are written in order of development tasks.

* A new module called PMAC is to be created, hosted on dls-controls github.
* The current type 3 driver present in the DLS tpmac module shall be moved into the PMAC module.
* Giles Knap's work on switching axes into and out of coordinate systems shall be applied to the PMAC module.
* A type 3 Coordinate System axes class shall be created, based on the current type 3 Coordinate System axes class present in the powerPMAC module.


