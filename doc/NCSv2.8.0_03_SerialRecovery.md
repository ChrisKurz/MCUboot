<sup>SDK version: NCS v2.8.0 - Link to Hands-on solution: [MCUboot1](https://github.com/ChrisKurz/MCUboot/tree/main/Workspace/NCSv2.8.0/03_SerialRecovery)</sup>

# MCUboot Hands-on:  Adding Serial Recovery to the Project

## Introduction

## Required Hardware/Software
- Development kit [nRF52840DK](https://www.nordicsemi.com/Products/Development-hardware/nRF52840-DK), [nRF52833DK](https://www.nordicsemi.com/Products/Development-hardware/nRF52833-DK), [nRF52DK](https://www.nordicsemi.com/Products/Development-hardware/nrf52-dk), or [nRF54L15DK](https://www.nordicsemi.com/Products/Development-hardware/nRF54L15-DK)
- install the _nRF Connect SDK_ v2.8.0 and _Visual Studio Code_. The installation process is described [here](https://academy.nordicsemi.com/courses/nrf-connect-sdk-fundamentals/lessons/lesson-1-nrf-connect-sdk-introduction/topic/exercise-1-1/).


## Hands-on step-by-step description 

### Enable Serial Recovery in MCUboot

#### Use previous project as starting point

1) Let's make a copy of the previously used [01_mcuboot1](MCUboot1](https://github.com/ChrisKurz/MCUboot/tree/main/Workspace/NCSv2.8.0/01_MCUboot1) project. Name this project _03_SerialRecovery_.
  
   > **NOTE:** For this project we also select _use sysbuild_ for system build when adding the Build Configuration.

2) Rename the image title in the prinf() instruction:

	<sup>_src/main.c_ => main() function</sup>

           printf("Image: Serial Recovery, version 1\n");


#### Configuration of MCUboot project

3) We want to put the configuration of the MCUboot project in our own project folder. This is supported by creating a __sysbuild__ folder and placing a __mcuboot.conf__ file into it. Your project folder should then look like this:

    _Workspace folder_/01_MCUboot1<br>
    |--- src<br>
    |--- |--- main.c<br>
    |--- CMakeLists.txt<br>
    |--- prj.conf<br>
    |--- sysbuild.conf<br>
    |--- sysbuild<br>
    |--- |--- mcuboot.conf

4) This step can be skipped, because the logging within MCUboot is activated by default. In case a custom board definition was used for your project. you should ensure that MCUboot logging is enabled. 
   
	<sup>_sysbuild/mcuboot.conf_</sup>

       CONFIG_LOG=y
       CONFIG_MCUBOOT_LOG_LEVEL_INF=y

#### Add Serial Recovery mode in MCUboot

5) Enable Serial Recovery by following KCONFIG setting in sysbzild/mcuboot.conf file.

	<sup>_sysbuild/mcuboot.conf_</sup>

       CONFIG_MCUBOOT_SERIAL=y
       CONFIG_BOOT_SERIAL_UART=y

6) Zephyr UART console must be disabled if Serial Recovery mode is used.

	<sup>_sysbuild/mcuboot.conf_</sup>

       CONFIG_UART_CONSOLE=n
 

7) MCUboot allows to indicate that MCUboot is in Serial Recovery mode via an LED. This feature can be enabled by setting following KCONFIG:

	<sup>_sysbuild/mcuboot.conf_</sup>

       CONFIG_MCUBOOT_INDICATION_LED=y

#### DeviceTree settings for MCUboot

8) Let's add an mcuboot.overlay file.
   
    _Workspace folder_/01_MCUboot1<br>
    |--- src<br>
    |--- |--- main.c<br>
    |--- CMakeLists.txt<br>
    |--- prj.conf<br>
    |--- sysbuild.conf<br>
    |--- sysbuild<br>
    |--- |--- mcuboot.conf<br>
    |--- |--- mcuboot.overlay   

9) Define the button and LED that will be used. 

	<sup>_sysbuild/mcuboot.overlay_</sup>

       / {
         aliases {
                 mcuboot-button0 = &button1;
                 mcuboot-led0 = &led1;
                 };
         };
   

### Testing

10) Build and flash the project to your board.
11) Hold __button 2__ while resetting the development kit. The kit will now enter Serial Recovery mode. LED2 is indicating this mode. 


### Add DFU over UART to the application

12) Add MCUMGR software module by adding following lines to our prj.conf file.

	<sup>_sysbuild/mcuboot.conf_</sup>

        # Enable MCUMGR
        CONFIG_MCUMGR=y

        # Enable MCUMRG management for both OS and Images
        CONFIG_MCUMGR_GRP_OS=y
        CONFIG_MCUMGR_GRP_IMG=y

        # Configure MCUMGR transport to UART
        CONFIG_MCUMGR_TRANSPORT_UART=y

        # Dependencies:
        # Configure dependencies for CONFIG_MCUMGR
        CONFIG_NET_BUF=y
        CONFIG_ZCBOR=y
        CONFIG_CRC=y

        # Configure dependencies for CONFIG_MCUMGR_GRP_IMG
        CONFIG_FLASH=y
        CONFIG_IMG_MANAGER=y

        # Configure dependencies for CONFIG_IMG_MANAGER
        CONFIG_STREAM_FLASH=y
        CONFIG_FLASH_MAP=y
    
        # Configure dependencies for CONFIG_MCUMGR_TRANSPORT_UART
        CONFIG_BASE64=y
    


