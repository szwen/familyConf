# familyConf
A family videoconference setup

## Intro
In the Covid-19 time, there have been (and may be) times when we can't be together with our families. Videoconferencing platforms have been highly used for this purpose, but as with any other technologies, it might be frequent that elder people struggle with it and can't participate leaving them isolated.

The purpose of this project is to set up a simple, low cost and easy to use videoconferencing system for my grandparents and any grandparents in the world, or whoever prefers to use their TV instead of a computer or a smartphone to speak with their loved ones.

It requires some preparation beforehand, but the idea is that once installed it is as easy to use as possible.

## Materials

- A raspberry pi - any of them. I chose raspi 3 even if 4 was already out since it is much cheaper and has ports compatible with older devices.
- A SD card and card reader for the set up of the raspi OS.
- A webcam
- A remote control (??)

## Setup

### 1. Raspberry initial setup
Install the Raspberry Pi OS: https://www.raspberrypi.org/downloads/raspberry-pi-os/
That should be a regular installation according to your raspi model

### 2. WEBCAM setup
Connect the webcam to the raspberry pi and run a few tests with the chosen videoconferencing platform to check that the hardware works fine.
This step heavily depends on the chosen platform. For example, we are going to use Google Meet from the Chromium browser, which required us to select the microphone source both in Chromium (chrome://settings/content/microphone) and in the Meet platform. We selected one of the entries named 'USB Audio-Hardware device'.

### 3. Videoconference autorun

Set up browser autorun for de videoconferencing: 
- Edit the autostart file, which might be in different paths depending on the OS version:
`nano /home/pi/.config/lxsession/LXDE-pi/autostart`
`nano /etc/xdg/lxsession/LXDE/autostart`
- Add `chromium-brower --start-fullscreen [path to the meeting]`
We chose path to the meeting to be: meet.google.com/meetingid

### 4. Remote control / troubleshooting

Follow the steps to enable VNC: https://www.raspberrypi.org/documentation/remote-access/vnc/
- Activate VNC server in the raspi
- Create a realVNC account to be able to connect to the raspi from another network


## Questions

Which videoconference mechanism?
*Hangouts:*
- How long do unique conference links last?
- How can I share a link with somebody?

