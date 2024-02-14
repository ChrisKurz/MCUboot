# MCUboot

This repository contains hands-on descriptions that show step-by-step how to integrate MCUboot into a custom project. 
Each individual hands-on focuses on specific features. 

Note that MCUboot does not handle the upgrade image download. This has to be handled in the application software. The Zephyr's _Device Managment_ includes _McuMgr_ support that helps with upgrade image download. 


## MCUboot Hands-on
In this subchapter we will focus on MCUboot. In these exercises, the firmware upgrade image is loaded via the Programmer tool. This helps to keep the project simple and we can focus only on using the MCUboot features. 

### Basics
- [Adding MCUboot to a project](doc/NCSv2.5.0_01-AddingMcubootToProject.md)
- [Generate a Key File](doc/NCSv2.3.0_GenerateKey.md) - A key file is needed when you want to generate your own signature key for an application image, or you would like to encrypt your upgrade image. This exercise describes how to generate a key file.
- [Create a signed Application Image](doc/NCSv2.3.0_ImageSigning_(ecdsa-p256).md) - using Signature Type _Elliptic curve digital signatures with curve P-256_

### Confirming an upgrade image
- [Create a confirmed upgrade image](doc/NCSv2.3.0_01a-SwapTypePermanent.md)

### Downgrade Prevention
- [Using Software-based Downgrade Prevention](doc/NCSv2.3.0_DowngradePrevention.1.md) - This method works only in image upgrade mode _Overwrite image updates instead of swapping_!


## McuMgr Hands-on
In this subsection we will look at how to handle the download of the upgrade image with _McuMgr_. 

### Bluetooth LE

- [Adding SMP Server with Bluetooth Transfer to a Project](doc/NCSv2.5.0_McuMgr_smp_ble.md) - Here we add device firmware upgrade in an application and perform the upgrade image download via Bluetooth.
