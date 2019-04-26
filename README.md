# QMK_Teensy_2.0_Tutorial
Basic steps necessary to put firmware on a Teensy 2.0

## Who this guide is for
This guide if for people who have finished the assembly process of their mechanical keyboards and are at the stage where you must put firmware on the Teensy. This will be written in the perspective of someone with a 4x12 grid layout keyboard, although any layout should be able to make use of this guide.

## Why did you write this when there's so many guides out there?
I wrote this guide because between the several tutorials out there, there's many inconsistencies and interlinking between guides. The goal of this tutorial is to have you up and running without two dozen tabs open afterwards. Also, I just wanted to type on my planck. That's the real reason.

## What you'll need
There are *several* different approaches to getting firmware onto your Teensy 2.0. My approach is just one of many. If you don't like it, that's totally okay! There's many other tutorials out there.

### On Windows
1. [Teensyloader](https://www.pjrc.com/teensy/loader.html) - This is what we'll use to put the firmware onto your keyboard.
2. [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - We'll set up an environment here using an Ubuntu disk image, where we'll be able to make our firmware without the headaches of trying to treat Windows like Linux.
3. [Ubuntu 16.04 LTS ISO file](https://www.ubuntu.com/download/desktop) - which Linux distribution you use is relatively trivial. If you're familiar with other deb-package based environment, feel free to use one of your choice. Don't worry about installing this yet, just get the .ISO file.
4. A text editor of your choice. That's it!

### On Mac
// TODO

### On Linux
1. [Teensyloader](http://www.pjrc.com/teensy/loader_linux.html) - This is what we'll use to put the firmware onto your keyboard.

## Getting the files
Download [Jack Humbert's fork of tmk_firmware](https://github.com/jackhumbert/qmk_firmware/archive/master.zip). Unzip this, and remember where you leave it.

## Creating your Virtual Machine environment
This may look long and intimidating. Don't be scared - I just included every detail I could in hopes that you won't get stuck.
- Open up VirtualBox
- Click `New`
- Give your virtual machine a name, like Cubert. Set your `type` to `Linux` and your `Version` to `Ubuntu (64-bit)` or `Ubuntu (32-bit)`, depending on which .ISO file you grabbed. If you gave it a sensible name like `Ubuntu`, it would set these values automatically. `Next`.
- Allocate however much memory you feel is appropriate. Whatever the recommended value is is probably sufficient, but if you bought 32GB of RAM in 2013 like I did, make sure to crank it up to 8 so 16GB just to try to convince your dying ego that it was worth it. `Next`.
- Close your eyes, press `Create Next Next`, jack up your file allocation size to 16GB (Ubuntu will want more than 8GB), and press `Next`.
- With your new virtual machine selected, press the big `Start` button. You'll be prompted to select a startup disk. Press the ![Choose .ISO icon](images/chooseiso.PNG) icon next to the dropdown and browse to find the .ISO file you downloaded from the Ubuntu website, and press `Start`.
- Your virtual machine should start booting up. I'm going to speedrun the Ubuntu installation because it's well documented, and most options won't affect our process.
 - `Install Ubuntu`
 - `Continue`
 - `Install Now`
 - `Continue` (Don't panic, it's going to overwrite that 16GB virtual disk we created)
 - The next few options are purely personal configuration and are totally up to you. Wait for it to finish installing. When it's finished - press `Restart Now`. You may need to power off the machine manually.
- Press the big green `Start` button again. We're now in our installed environment. First things first, let's install the VirtualBox Guest Additions. This will make using the VM a little easier by allowing the screen to dynamically resize and whatnot. Click `Devices` and `Install Guest Additions CD image...`. This will cause a dialog box to pop up in your virtual machine. Press `Run`. You'll need to enter your password, and allow the installation to take place. Afterwards, shut down the machine.
- While your VM is shutdown, press the big yellow `Settings` cog. In the left hand pane, select `Shared Folders`. Press the new file button, browse to the `qmk_firmware` folder that you unzipped earlier, check the `Auto-mount` checkbox, and press `OK` on both dialogs. Again, start the machine.
- Press `Ctrl + Alt + T` to bring up the command prompt.
 - Run `sudo usermod -a -G vboxsf <your username>`. This adds your user to a group, which grants your Ubuntu user account permission to access the folder that we shared with our virtual machine a few steps above.
- Restart your virtual machine once more.
- Log back in, press `Ctrl + Alt + T`, and run this command: `sudo apt-get install gcc-avr binutils-avr gdb-avr avr-libc avrdude`. This will install the AVR GCC Toolchain.

You're finally ready!

This is a pretty big package, and we're barely going to scratch the surface. Enter the `keyboard` directory. You'll see many different titled keyboards. In this tutorial, we'll use `planck`, although the process should be similar for any layout.

### config.h
The first file we'll mess with is the `config.h` file of whatever directory you've chosen. We're going to change two lines - the `COLS` and `ROWS` definitions.

    #define COLS (int []){ C7, C6, D7, B4, B5, B6, F7, F6, F5, F4, F1, F0 }
    #define ROWS (int []){ B3, B2, B1, B0 }

Above is my configuration. Most likely, yours is different. These codes represent the pins used on my Teensy 2.0 for each row and column. The pinout numbers are shown in the image below.

![Teensy 2.0 pinouts](images/pinout.png)

**Remember** that when you have your keyboard upside down...that it's upside down. Pinout rows are listed from top to bottom, and columns are listed left to right. If you have a different number of rows or columns than I do, scroll up and make sure your `MATRIX_ROWS` and `MATRIX_COLS` values are correct.

Feel free to spend some time getting to know this file for the other configuration it allows.

### Your own firmware

Until I find the time to write this section in my own tongue, [this guide](https://github.com/etergo/JH-TMK-Tutorial/blob/master/TUTORIAL.md) should be beneficial to look at.
