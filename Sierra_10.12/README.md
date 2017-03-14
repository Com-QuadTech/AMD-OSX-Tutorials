# Installing and setting up `macOS Sierra (10.12)` on AMD-based PCs
###### Written by IncognitoJam (In progress)
###### You can find the latest version of this tutorial [here](https://github.com/IncognitoJam/AMD-OSX-Tutorials/blob/master/SIERRA%2010.12/SIERRA%2010.12%20TUTORIAL.md). Although I am sorry if mistakes were made in this tutorial, I cannot be blamed for any lasting impact it has on your computer. You have been warned.

This is a brief tutorial to help people get started running macOS Sierra. It will show you how to create a USB installer for Sierra and use this to install Sierra to a drive on your computer. 

In future I will expand this tutorial to also show how to install various other things such as NVIDIA GeForce web drivers, audio drivers and ethernet drivers. I will also add a section to show how to install Clover to your USB or hard drive.

#### Prologue:
Your computer specifications play a large role in how likely it is you will be able to install Sierra. I was very lucky with the parts I have and it made installing Sierra that it was for others. You can view my computer parts [here](https://uk.pcpartpicker.com/user/Jamboozlez/saved/HpDK8d). In brief:
```
CPU: AMD FX-6300
MOBO: ASRock 980DE3/U3S3 AM3+
GPU: MSI GeForce GTX 950 2GB
```


### Requirements
1. You are running Windows (or macOS, see Part 1) on an AMD based PC.
2. You have an empty USB which is at least `12GB` in size.
3. You have another hard drive within your computer which you can install macOS to. I will not show you how to install macOS to the same drive (on a different partition) and I would not recommend overwriting Windows as you may have to go back to it if you ruin your USB installer.
3. You have necessary backups of important files - don't make the same mistake as me. Don't store these backups on your computer - upload them to Google Drive or Dropbox.
4. You have a brain and the ability to use some common sense. This tutorial won't tell you every button to press or every letter to type. If you aren't able to act independently this probably isn't the best tutorial for you.

I would also recommend that you either print this document, or have a smartphone or another computer at hand to read this whilst you install Sierra.

### Downloads
- Thank you to Shaneee for his extremely useful Sierra image, although it isn't perfect! 😜  I have modified his veyr useful installer image to include the correct kexts for USB to help prevent errors, and I have also provided a Post Install script to automate the second part of the installation process. Please follow this [link]() to download the torrent I have created to share this installer image. The file should be named `IncognitoSierraAMD.dmg`. 
- If you are using Windows you must also download and install [TransMac](http://www.acutesystems.com/tmac/tmsetup.exe) as we wil use this to format and restore your USB with the Sierra image.

## 1. Creating the Installer

If you are using a Mac to create the installer, you can use Disk Utility to restore the image to your USB and then proceed with the installation from Part 2.

**On Windows:**
1) Open **TransMac** by right-clicking on it in the start menu and selecting **Run as administrator**. This will allow you to restore the USB with the `.dmg` file you have downloaded.
2) You should be able to see your USB in the left pane. Right-click on your USB and select `Restore with image` (or similar) and then select the image you just downloaded. This will begin the image restore process which may typically takes around 30 minutes.
3) Once this completes, you are finished in Windows! You can shut down your computer.

## 2. Preparing your BIOS
Turn on your computer again but you will need to boot into your BIOS. On my ASRock motherboard I do this by pressing either `F2` or `Del` however this will vary by manufactuer. You can either Google search the key for your motherboard, or it may be shown on your monitor during the boot process. Press or hold down the key whilst your computer is booting.

- To prevent issues later with USB mouse/keyboard drivers, you should disable `USB 3.0 controller` in your BIOS if your motherboard supports USB 3.0.
- You should also make sure that `Secure Boot` is disabled if this option is present.
- I'm sure I should be telling you to either enable or disable `UEFI` or Legacy Mode, but I don't know which or how, so this is a warning that you should probably do something about it but I don't know what :) My motherboard doesn't support `UEFI` so I didn't have to make that decision, however it did complicate the installation of a bootloader such as `Clover` which is intended for `UEFI` systems. I will explain this in more detail later in the tutorial.
- Whilst you're here in the BIOS, you should also change your `Boot Order` to prioritise attached USB drives over your disk drives. This will allow you to boot the USB without having to hit `F11` or any other key combination to access your installer.

## 3.1 Installing macOS Sierra: Formatting Disk and Installing OS
1. Start up your computer and boot from the USB (it should do this automatically if it is still plugged in and you changed your `Boot Order`).
2. After a minute your computer should reach a screen which shows a list of disk volumes, including one labelled `IncognitoSierraAMD` (or similar). Using your arrow keys highlight this volume and simply press enter. You may have noticed that the bootloader allowed boot arguments to be entered, but these should not be required at this stage of the installation*.
3. You have now loading the macOS Sierra installer! Select your language and click continue.
4. Don't click continue again, but instead press `CTRL + F2` to highlight the options bar at the top of your screen and use the arrow keys to navigate to `Utilities -> Disk Utility` and press enter to open the Disk Utility.
5. Once Disk Utility has opened, press the up arrow key to highlight drives in the left pane and select the hard drive you wish to install macOS to. Select `Erase` at the top of the window and choose the following options:
- **Name:** You must call this `OSX` for the installer script to work.
- **Format:** `Mac OS Extended (Journaled)` - Do not select encrypted or case sensitive as some encryption is unlikely to work on a hackintosh and case sensitive can cause issues with scripts.
- **Scheme:** `GUID Partition Map` (everything else is outdated)
6. Click erase and once the procedure finished press `Windows Key + Q` (or `Command + Q`) to close Disk Utility.
7. Now that you are back to the installer, click continue, select `OSX` as your installation drive by double left-clicking it and click continue again to begin the installation of macOS Sierra.
8. Allow the installation to finish and do not remove the USB as it will be used in the second part of the installation.

## 3.2 Installing macOS Sierra: Post Install Process
1. Now that the install has finished your computer should have rebooted and you should be back to the boot menu once again (as in step 2 of part 1). Boot to the one labelled `IncognitoSierraAMD` again by selecting it with the arrow keys and pressing enter.
2. Press continue and then `CTRL + F2` just as before but this time navigate to `Utilities -> Terminal` and press enter.
3. In the terminal type the following commands:
```
    cd /Volumes/IncognitoSierraAMD/PostInstall
    sudo sh install.sh
```
This will begin the automated installation process to apply the required kexts (that is, kernel extensions) for your system to function correctly running macOS.

4. Once this has finished you can close the terminal with `Windows Key + Q` (or `Command + Q`), and press the shortcut again to close the installer. Choose `Restart` on the popup to boot, again, to the USB you created.

## 3.3 Installing macOS Sierra: First Boot and User Creation
1. You should now be at the bootloader once more. You have already installed the operating system to your preferred drive and loaded the required kexts into the kextcache so that your system and kernel can communicate with your computers peripherals. However, you may also need to provide `Boot Flags` to get your system to run correctly.
### Boot flags

- `-v`: This enables **Verbose Mode** so that your system logs are displayed instead of the boot animation/apple logo whilst the operating system is loading. It is recommended to use this flag so that if an error occurs you may be able to read what it is (before your computer restarts itself!) and this information will help to diagnose the problem. If you are unsure what the error is take a photo of the error or record the boot on a video. Someone else may be able to help you on the forums or in the AMD OS X discord server.
- `-x`: This enables **Safe Mode** which causes macOS to ignore all non-critical kext files and boot the system with limited functionality. This is useful if you installed a faulty kext and it is causing your system to crash.
- `-UseKernelCache=No`: This causes your mac to ignore **cached kexts** (which would usually cause your system to boot faster) and re-read them from the /S/L/E directory. This is useful if you did not install a kext correctly.
- `PCIRootUID=1`: If you use an **AMD Radeon graphics card** this is sometimes a required flag so that it functions correctly. Alternatively, you may need to use `PCIRootUID=0` for the App Store to become verified.
- `npci=0x2000` or `npci=0x3000`: These are required flags if your system hangs during boot with the message `[PCI Configuration Begin]`. You should usually use `npci=0x3000` but you can also try the other flag if it does not help.
- There are also **many other flags** which you may find are useful when trying too boot your system such as `NvidiaWeb=1` to enable NVIDIA web drivers (I will cover this later) or `GraphicsEnabler=No` if your NVIDIA card is causing issues during the setup process. You can ask for help choosing boot flags on the forums or in the AMD OS X discord server.

I used the following flags to boot my system, however they will vary largely depending on what components you have in your hackintosh.
```
-v npci=0x3000
```
2. Navigate to the drive you installed macOS to using the arrow keys (`OSX`), enter your boot flags and press enter to begin the first boot. This usually takes a while. If your system reboots try again but experiment with some flags which you think may be causing a problem. I recommend that you take a photo or record the boot (with `-v` enabled) so that you can ask others to help diagnose the problem.
3. Eventually you will reach the first setup. You will choose your network settings, user account details and some other Apple options. When asked about your network connections, your built in Ethernet or Wi-Fi may not work correctly. In this case if you choose to use either of these your system may reboot as the required drivers/kexts were not found. I had to choose 'No internet' (or similar) and I installed internet drivers later on.
4. Once this installation has finished your computer will reboot (or you will have to restart it). You can now boot to the same `OSX` drive to load macOS Sierra for the first time.

## 4. Setting up macOS Sierra
Once you login you will notice that there may be issues with graphics drivers if you are not using a vanilla supported graphics card, such as a NVIDIA card (you will have a low screen resolution or icons will be green). Other issues include no sound from your speakers or no internet access. These can be fixed by installing kexts or driver packages.

Also, you will want to install a bootloader so that you can access macOS without using the USB installer you created (however you can install a bootloader to the USB also, if you would prefer to have that option as I do). The bootloader can be configured to handle the boot arguments for you and will allow you to access Windows or other operating systems if you decide to set your macOS drive as the default in your BIOS `Boot Order`. 

### Installing a bootloader (Clover)
For now I will only show how to install the **Clover** bootloader as this is the more modern choice and was rewritten to resolve issues with the App Store and iCloud verification (making your life easier later on). 

I have provided the latest installer package for Clover (at the time of writing) as well as the `Clover Configurator` app in the `/PostInstall/` directory of the installation media you create. Below I will explain how to install Clover as well as how to use the configurator app to change boot arguments among other things.

1. Firstly open Finder and navigate to your `IncognitoSierraAMD` drive and open the `PostInstall` directory. 
2. Drag and copy the `DRAG TO DESKTOP` files.. to your desktop.
3. Open the directory and then open the `CLOVER Files` directory.
3. Whilst holding your `CTRL` key, Left-click the `Clover_v2.4k_r4035.pkg` file and then click open on both prompts to begin the Clover install process. **Do not** open Clover Configurator yet.
4. *To be completed*

### Installing NVIDIA Web Drivers
I have provided the latest NVIDIA drivers for macOS Sierra 10.12 on the installation media you created in the `/PostInstall/` directory. You will use this package to install the correct graphics drivers. These drivers will work on GTX 9xx series cards. GTX 10xx cards are currently not supported as Apple has not yet allowed NVIDIA to release their drivers for macOS.

1. *To be completed*
