<sup>SDK version: NCS v2.8.0 - Link to Hands-on solution: [MCUboot1](https://github.com/ChrisKurz/MCUboot/tree/main/Workspace/NCSv2.8.0/04_extFlash)</sup>

# MCUboot Hands-on:  Adding external Flash

## Introduction
MCUboot uses a dual-slot solution for upgrading a fimrware. slot 0 is used to execute the code, while slot 1 is used for the upgrade image. As soon as the complete upgrade image was downloaded, a reset will cause that MCUboot is executed that detects the complete, signed imagea and moves it to slot 0. Sometimes it makes sense to place external Flash on the PCB and connect it via the QSPI interface to the nRF52, nRF53, or nRF54 MCU. And this is what we will do here.


## Required Hardware/Software
- Development kit [nRF52840DK](https://www.nordicsemi.com/Products/Development-hardware/nRF52840-DK), [nRF52833DK](https://www.nordicsemi.com/Products/Development-hardware/nRF52833-DK), [nRF52DK](https://www.nordicsemi.com/Products/Development-hardware/nrf52-dk), or [nRF54L15DK](https://www.nordicsemi.com/Products/Development-hardware/nRF54L15-DK)
- install the _nRF Connect SDK_ v2.8.0 and _Visual Studio Code_. The installation process is described [here](https://academy.nordicsemi.com/courses/nrf-connect-sdk-fundamentals/lessons/lesson-1-nrf-connect-sdk-introduction/topic/exercise-1-1/).


## Hands-on step-by-step description 

#### Use previous project as starting point

1) Let's make a copy of the previously used [03_SerialRecovery](MCUboot1](https://github.com/ChrisKurz/MCUboot/tree/main/Workspace/NCSv2.8.0/03_SerialRecovery) project. Name this project _04_ExtFlash_.
  
   > **NOTE:** For this project we also select _use sysbuild_ for system build when adding the Build Configuration.

2) Rename the image title in the prinf() instruction:

	<sup>_src/main.c_ => main() function</sup>

           printf("Image: Ext FLash, version 1\n");

#### Add QSPI driver to the project

3) Choosing a value for DTS _nordic,pm-flash_ property allows to store partitions in the external flash. This is done by setting the _nordic,pm-flash = &mx25r64_. This has to be done in the application overlay file as well as in the MCUboot overlay file. We have already added the mcuboot.overlay file before. But we also need the overlay file for the application. Here we use the board name and as extension __.overlay__. The file structure of our project should then look like this:

    _Workspace folder_/01_MCUboot1<br>
    |--- src<br>
    |--- |--- main.c<br>
    |--- CMakeLists.txt<br>
    |--- prj.conf<br>
    |--- sysbuild.conf<br>
    |--- **nrf52840DK_nrf52840.overlay**<br>
    |--- sysbuild<br>
    |--- |--- mcuboot.conf<br>
    |--- |--- **mcuboot.overlay** 

   > **NOTE**: in this project the NRF52840DK is used. Because of this the overlay file is named __nrf52840dk_nrf52840.overlay__. In case another DK is used, the overlay file name has to be accordinglychanged.

4) Add following lines to both overlay files, **nrf52840dk_nrf52840.overlay** and **sysbuild/mcuboot.overlay**.

	<sup>_nrf52840DK_nrf52840.overlay_, and _sysbuild/mcuboot.overlay_</sup>

       / {
          chosen {
                  nordic,pm-ext-flash = &mx25r64;
                 };
         };

5) We add the QSPI driver to the MCUboot project.

	<sup>_sysbuild/mcuboot.conf_</sup>

       CONFIG_NORDIC_QSPI_NOR=y

6) The number of sectors used by MCUboot needs to be increased.

	<sup>_sysbuild/mcuboot.conf_</sup>

       CONFIG_BOOT_MAX_IMG_SECTORS=256

