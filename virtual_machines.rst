===============================
How to Create a Virtual Machine
===============================

.. contents::
   :local:

Why create a VM?
================

Advantages
----------

* Can test software installations in a sandbox before use, can revert changes if it doesn't work
* Can give the virtual machine with the software to the client (so they don't have to install anything to run the program)

Disadvantages
-------------

* Need enough RAM: need twice the amount of memory of the VMs
* Have to have enough disk space
* Have to have 64-bit CPU with virtualisation extensions

Process of Creating a VM
========================

Download Disk Image
-------------------

* Download the .iso file (64 bit) e.g. xubuntu-14.04.2-desktop-amd64.iso

`Xubuntu Desktop <http://cdimages.ubuntu.com/xubuntu/releases/14.04/release/>`_

* Why Xubuntu:

 - It's very lightweight

Create a New VirtualBox
-----------------------

* Download and Install Oracle VirtualBox:

`Oracle VirtualBox <https://www.virtualbox.org/wiki/Downloads>`_

* Startup VirtualBox and create a new VM e.g.

 - Name: Xubuntu 14.04.2 LTS
 - Type: Linux 
 - Version: Xubuntu 64 bit

Create Virtual Hard Drive
-------------------------

* Memory size (half of what is installed): 6GB
* Select option: Create a virtual harddrive now
* Select option: VirtualBox Disk Image (experience is that the other options don't work any better)
* Select option: Fixed size 10GB
* Hit "Create" (might take about 10 minutes or so)

You now have a virtual harddrive

Install Operating System
------------------------

* In VirtualBox > Settings > Storage > IDE (Empty)

  - Controller: IDE
  - Choose Ubuntu ISO Image

* In VirtualBox > Start 

 - Select: Install Ubuntu
 - Don't Select: Download updates while installing
 - Gave username and password

 - Select: Continue
 - Select: Install Now
 - Where are you? UK
 - Detect Keyboard
 - Continue
 - When installation is complete:

   + Select: Restart Now
   + Ensure CD is removed: Press Enter
   + It should restart (you may have to shutdown instead or power off machine)

**Snapshot taken at this point**

Change the Settings of the Virtual Machine
------------------------------------------

* In VirtualBox > Settings > System

  - 12MB or (64MB Video Memory)
  - Processor: change to 3 (needs virtualisation extensions)
  - Execution Cap: 100%

Run Updates for Xubuntu
-----------------------

* In VirtualBox > Start 
* Run updates for Ubuntu

::

    $ sudo apt-get update

**Snapshot taken at this point**

Install DKMS
------------

* Install Dynamic Kernel Module Support – otherwise everytime you update the kernel, you'll have to update Guest Additions

::

    $ sudo apt-get install dkms

Install VirtualBox Guest Additions
----------------------------------

* Install Guest Additions, which gives enhanced mouse pointer and video control and installs Vbox Service.

* My version of VirtualBox needed these installing before Guest Additions:

::

    $ sudo apt-get install linux-headers-generic

    $ sudo apt-get install xserver-xorg xserver-xorg-core

* Then Devices > Install Guest Additions (should open file system with CD) - use version that came with VirtualBox, not the version that is in the respository of Ubuntu
* This should appear on the desktop: VOXADDITIONS_4.3.12_93733

::

    $ cd /media/andrew/VOXADDITIONS_4.3.12_93733

    $ sudo sh ./VboxLinuxAdditions.run

* Eject VM Guest Additions from File Manager
* Restart Ubuntu Guest

**Snapshot taken at this point**

Pass Through Folder
===================

* There are loads of tutorials on this, the best on is this:

`Pass Through Folder <https://www.youtube.com/watch?v=1iUafW6g5o8>`_

* Obtain the name of the Ubuntu Box:

::

   sudo nano /etc/hosts

(Could be VMUbuntu)

* In Xubuntu Guest Machine

::

    $ sudo apt-get update
    $ sudo apt-get samba

* In File Manager: Right click folder > Share Folder (will install Samba) 

* Restart session

* In Ubuntu Guest Machine

::

    $ sudo gedit /etc/samba/smb.conf &

* In that file add a line:

::

    [global]

    workgroup = WORKGROUP
    force user = username (this is the username in Ubuntu VM)

* Restart Samba:

::

    $ sudo restart smbd

* Right click folder > Share Folder (Allow others to create and delete files).

 - Create share
 - Create a file in Ubuntu to share

* In Windows Search:

::

    \\VMUbuntu\Public

* In Windows Explorer > Network (May need to turn on share and discovery)

How to Fix Host/Guest Copy Paste Issues
=======================================

* In Virtual Box:

  - Settings > General > Advanced

     + Shared Clipboard: Bidirectional
     + Drag n' Drop: Bidirectional

Snapshot Management
====================

* Useful for trial software installation. Remember: just running an operating system creates changes. We can't restore while the machine is powered on.

* In VirtualBox > Snapshots:

  - Right click > Take snapshop
  - Right click > Restore snapshot 
  - Right click > Delete snapshot (to remove large files)

How to Clone VMs
================

* How to give other people the VM? **Clone it** Can't give just snapshot

* Clone snapshot: Right click > Clone

**If you are going to be using the clone on the same network as the virtual machine check reinitilise the MAC address of all network cards – otherwise you will have multiple machines with the same MAC address and this will confuse the network.**

* Select: Full clone – so other people can use it
* Select: Clone everything

Can now give other people the .vdi file (the harddisk) and the .vbox. They can launch this directly in their copy of virtual box.

Adjust Network Adapter
======================

* Use: NAT (allows Guest to use Internet)

* Adapter Type: PCnet-Fast III (Am79C973)

* Can randomise MAC address. 08002731A607 (if other VMs are somehow using the same address)

Types of Storage
================

* In VirtualBox: Settings > Storage
* Ubuntu uses SATA, CD uses IDE.

How to Copy vdi files
=====================

* File > Virtual Media Manager
* Differencing files are roughly equivalent to snapshots
* .vdi files have unique identifiers – can't just copy and paste
* **If you want to copy a snapshot – you must use clone (as above)**
* If you want to copy a VM (that isn't a snapshot), you can use: File > Virtual Media Manager > Copy > Next > Next > Next > Give it a name > Copy
* To test the copy, you must add the .vdi file as a **harddrive to a virtual machine** (not just in the virtual machine manager)

