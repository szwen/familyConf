# familyConf
A family videoconference setup

## Intro
In the Covid-19 time, there have been (and may be) times when we can't be together with our families. Videoconferencing platforms have been highly used for this purpose, but as with any other new technologies, it might be frequent that elder people struggle with it and can't participate, which leaves them isolated.

The purpose of this project is to set up a simple, low cost and easy to use videoconferencing system for my grandparents and any grandparents in the world, or whoever prefers to use their TV instead of a computer or a smartphone to speak with their loved ones.

It requires some preparation beforehand, but the idea is that once installed it is as easy to use as possible.

## Materials

- A raspberry pi - any of them. I chose raspi 3 even if 4 was already out since it is much cheaper and has ports compatible with older devices.
- A SD card and card reader for the set up of the raspi OS.
- A webcam
- A remote control (??)

## Setup

### 1. Raspberry initial setup
[Install](https://www.raspberrypi.org/downloads/raspberry-pi-os/) the Raspberry Pi OS. That should be a regular installation according to your raspi model

### 2. WEBCAM setup
Connect the webcam to the raspberry pi and run a few tests with the chosen videoconferencing platform to check that the hardware works fine.

This step heavily depends on the chosen platform. For example, we are going to use Google Meet from the Chromium browser, which required us to select the microphone source both in Chromium settings (chrome://settings/content/microphone) and in the Meet platform. We selected an entry named 'USB Audio-Hardware device'.


### 3. Videoconference autorun
The main idea of this step is to enable the Raspi to:
- Take over the TV channel when it starts up
- Enter the videoconference call automatically

#### 3.1 Raspi takeover of the TV (depends on the TV model)
Most modern TVs have support for the CEC (Consumer Electronics Control) feature of HDMI. This means that a device connected through HDMI to our TV can control it, for example turning it on and off, or switching the TV source. This way, we can force that when the Raspi is turned on the TV source automatically switches to show the Raspi screen.

Note that, depending on the TV brand, CEC capabilities must be enabled using the advanced TV menu, and for this you need to find the specific instructions (for example [this](https://www.tomsguide.com/us/lg-tv-settings-guide,review-5624-14.html) for LG TVs).

Some useful instructions obtained from [here](https://www.linuxuprising.com/2019/07/raspberry-pi-power-on-off-tv-connected.html) to test that this works with your TV:

1. Install CEC-utils in your Raspi: `sudo apt install cec-utils`
2. Find connected devices with CEC support (note that you need to connect the Raspi to your TV with the HDMI beforehand): `echo 'scan' | cec-client -s -d 1`

Given that you have a TV with CEC capabilities enabled, we'll configure the autostart Raspi menu to take over the TV as the active input source when it is turned on by adding this line:
`echo 'as' | cec-client -s -d 1`

Optionally you can (previously) make it turn on the TV by adding this line:
`echo 'on <DEVICE #>' | cec-client -s -d 1`

Where *<DEVICE #>* should be replaced with the device number assigned to the TV, which you saw when you run the previous `scan` command.

In summary, the autostart configuration goes as follows:
1. Create a script in your home directory with the following contents:
```
run_cec.sh:

#!/bin/bash

echo 'as' | cec-client -s -d 1
```

Make it executable by running `chmod +x run_cec.sh`


2. Edit the autostart file, which might be in different paths depending on the OS version:
* `nano /home/pi/.config/lxsession/LXDE-pi/autostart`
* `nano /etc/xdg/lxsession/LXDE/autostart`
* `nano /etc/xdg/lxsession/LXDE-pi/autostart`

Add the @lxterminal step to spawn a terminal on startup and pass it the script:

`@lxterminal --command "/home/pi/run_cec.sh"`


**IR Alternative**

Alternatively, you can transform your Raspi into a TV controller using a IR transceiver. We won't dive into this solution here, but there are some interesting pages [here](https://www.raspberry-pi-geek.com/Archive/2015/10/Raspberry-Pi-IR-remote), [here](http://opensourceuniversalremote.com/) and [here](https://raspberrypi.stackexchange.com/questions/22433/what-hardware-do-i-need-to-turn-raspberry-pi-into-a-tv-remote-controller)

**Manual Alternative**

In case the TV doesn't have CEC capabilities and you don't want to deal with LIRC, we can leave it up to the user to change the TV source to the HDMI port where the Raspi is connected to. 

#### 3.2 Videoconference autorun setup
Automatically open the browser with the meeting link and join it: 

1. Edit the autostart file, which (as we said above) might be in different paths depending on the OS version.

2. Add a step to open chrome with the selected link: `@chromium-browser --start-fullscreen [path to the meeting]`

We chose the path to the videoconference: `meet.google.com/<MEETINGID>`

Due to the requirements of Google Meet, we need to set up a gmail account in the Raspi, as well as to send an invite to the videoconference to it.
  
3. Join the meeting: in all the meeting programs we have seen, there is always a last step of joining the meeting by clicking a button. To automate it we propose Selenium, which allows to use hooks in the web app. https://www.selenium.dev/documentation/en/webdriver/
__note: it seems that buttons in google meet are obfuscated, it may be easier with meet.jit.si for example__

### 4. Remote control / troubleshooting

This might be the best (although not ideal) solution for updating configurations, setting up the camera drivers, as well as for starting the meeting remotely if needed. In case we use a this remote solution to run the meetings, we'll still need **step 3.1** (automated or manual) in order to choose the correct source for the TV, however we can skip **step 3.2**.

Follow the steps to enable VNC: https://www.raspberrypi.org/documentation/remote-access/vnc/
- Activate VNC server in the Raspi
- Create a realVNC account to be able to connect to the Raspi from another network
- [Install the VNC client in the controller device](https://www.realvnc.com/en/connect/download/viewer/macos/) and start managing the Raspi remotely!

## Questions

Which videoconference mechanism?
*Google meet:*
- How long do unique conference links last?


