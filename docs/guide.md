---
layout: default
title: User Guide
nav_order: 2
---

__FLibGoLite__ program is written in GO as a single executable and doesn't require any prereqiusites.  
__All you have to do is to download, install and start it.__

##  Download
[Download latest release](https://github.com/vinser/flibgolite/releases/tag/v2.0.0) of specific program build for your OS and CPU type  

|OS        |CPU type              |Program executable          |Tested<sup>1</sup> |  
|----------|----------------------|----------------------------|:------:|  
|Windows   | Intel, AMD 64-bit    | flibgolite-linux-amd64.exe |Yes     |  
|OS X (MAC)| Intel, AMD 64-bit    | flibgolite-darwin-64       |No      |  
|OS X (MAC)| ARM 64-bit           | flibgolite-darwin-64       |No      |  
|Linux     | Intel, AMD 64-bit    | flibgolite-linux-amd64     |No      |  
|Linux     | ARM 32-bit (armhf)   | flibgolite-linux-arm-6     |Yes     |  
|Linux     | ARM 64-bit (armv8)   | flibgolite-linux-arm64     |Yes     |  

<sup>1</sup>_Some of executables was only cross-builded and not tested on real desktops, but you can still try them out_  

You may rename downloaded program executable to `flibgolite` or any other name you want.
For convenience, `flibgolite` name will be used below in this README.

## Install and start
Although __FLibGoLite__ program can be run from command line, the preferred setup is program to be installed as a system service running in background that will automaticaly start after power on or reboot.

Service installation and control requires administrator rights.

On Windows open Powershell as Administrator and run commands to install, start and check service status

1. In Windows Powershell terminal run command

Install service:
```sh
  ./flibgolite -service install
```
Start service
```sh
  ./flibgolite -service start
```
And check that service is running
```sh
  ./flibgolite -service status
```

2. On Linux open terminal and run commands using `sudo`:

```bash
  sudo ./flibgolite -service install
  sudo ./flibgolite -service start
  sudo ./flibgolite -service status
```

If status is like "running" you can start to use it.

## Use
At the first run program will create the set of subfolders in the folder where program is located

 	flibgolite
	├─┬─ books  
	| ├─── stock - library book files and archives are stored here
	| └─── trash - files with processing errors will go here
	├─┬─ config - contains main configuration file config.yml and genre tree file
	| └─── locales - subfolder for localization files 
	├─── dbdata - database with book index resides here
	└─── logs - scan and opds rotating logs are here

Put your book files or book file zip archives in `books/stock` folder and start to setup bookreader. Meanwhile book descriptions will be added to book index of OPDS-catalog.

Set bookreader opds-catalog to `http://<PC_name or PC_IP_address>:8085/opds` to choose and download books on your device to read. See bookreader manual/help.

`Tip:` While searching book in bookreader use native keyboard layout for choosed language to fill search pattern. For example, don't use Latin English "i" instead of Cyrillic Ukrainian "i", because it's not the same Unicode symbol. 
