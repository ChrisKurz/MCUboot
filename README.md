# MCUboot

This repository contains hands-on descriptions that show step-by-step how to integrate MCUboot into a custom project. 

Note that MCUboot does not handle the upgrade image download. This has to be handled in the application software. The Zephyr's _Device Managment_ includes _McuMgr_ support that helps with upgrade image download. 

### Basics - Adding MCUboot, degining an own signature, and add first simple DFU
1. [Adding MCUboot to a project](doc/NCSv2.8.0_01-AddingMcubootToProject.md) - This chapter explains how to add MCUboot to your own application project and do a multi-image build. So you get a merged image that contains your application image and the mcuboot image.
2. [Generate a Key File](doc/NCSv2.8.0_02_GenerateKey.md) - A key file is needed when you want to generate your own signature key for an application image, or you would like to encrypt your upgrade image. This exercise describes how to generate a key file.
3. [Add Serial Recovery to the project]() - MCUboot itself supports firmware update via UART interface. So the application firmware does not need to handle firmware download in this case. 



-----------------------------------------------------------------------------------------------------------------------

## work in progess! The following links are for NCS v2.5.2 or older!!!

- [Create a signed Application Image](doc/NCSv2.5.2_ImageSigning_(ecdsa-p256).md) - using Signature Type _Elliptic curve digital signatures with curve P-256_

### Confirming an upgrade image
- [Create a confirmed upgrade image](doc/NCSv2.5.2_01a-SwapTypePermanent.md)

### Downgrade Prevention
- [Using Software-based Downgrade Prevention](doc/NCSv2.3.0_DowngradePrevention.1.md) - This method works only in image upgrade mode _Overwrite image updates instead of swapping_!


## McuMgr Hands-on
In this subsection we will look at how to handle the download of the upgrade image with _McuMgr_. 

### Bluetooth LE

- [Adding SMP Server with Bluetooth Transfer to a Project](doc/NCSv2.5.0_McuMgr_smp_ble.md) - Here we add device firmware upgrade in an application and perform the upgrade image download via Bluetooth.
