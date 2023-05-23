# MCUboot

This repository contains hands-on descriptions that show step-by-step how to integrate MCUboot into a custom project. 
Each individual hands-on focuses on specific features. 

Note that MCUboot does not handle the upgrade image download. This has to be handled in the application software. The Zephyr's _Device Managment_ includes _McuMgr_ support that helps with upgrade image download. 


## MCUboot Hands-on
In this subchapter we will focus on MCUboot. In these exercises, the firmware upgrade image is loaded via the Programmer tool. This helps to keep the project simple and we can focus only on using the MCUboot features. 

1) [Adding MCUboot to a project](doc/NCSv2.3.0_01-AddingMcubootToProject.md)
2) [Create a confirmed upgrade image](doc/NCSv2.3.0_01a-SwapTypePermanent.md)
3) [Using Software-based Downgrade Prevention](doc/NCSv2.3.0_DowngradePrevention.1.md)


## McuMgr Hands-on
In this subsection we will look at how to handle the download of the upgrade image with _McuMgr_. 
