# musicbox

Some scripts to create a RPI musicbox with a [HifiBerry DAC](https://www.hifiberry.com/) on Raspberry PI (especially [PI Zero](https://www.raspberrypi.org/products/raspberry-pi-zero/)) with Raspbian

- 26.04.2020 - now musicbox is compatible with Raspian Buster and RPI 4b

## Included software

- Spotify Connect Server: [spotifyd](https://github.com/Spotifyd/spotifyd)
- Plex Audio Player: [Plexamp for RPI v2.0 beta 2](https://forums.plex.tv/t/plexamp-for-raspberry-pi-release-notes/368282) - [Plexamp](https://plexamp.com/)
- Music Player Daemon (MPD) with TuneIn and Spotify Integration: [mopidy](https://www.mopidy.com/)
- Bluetooth Audio: [bluealsa](https://github.com/Arkq/bluez-alsa)
- Zeroconf: [avahi-daemon](https://www.avahi.org/)
- UPnP: [upmpdcli](https://www.lesbonscomptes.com/upmpdcli/) - UPnP Audio Media Renderer based on MPD

## Inspired by

THX to
- @nicokaiser for his scripts in [nicokaiser/rpi-audio-receiver](https://github.com/nicokaiser/rpi-audio-receiver) (Bluetooth and Spotifyd)
- @woutervanwijk and contributors for the [Pi MusicBox](https://www.pimusicbox.com) [project](https://github.com/pimusicbox/pimusicbox)

## Setup

- Install [Raspbian Lite](https://www.raspberrypi.org/downloads/raspbian/) on a SD card
- Insert SD card into your PI (Zero), connect HDMI and a keyboard
- Power up PI (Zero)
- Login with `pi` / `raspberry`
- Configure RPI
  - `sudo raspi-config`
  - Password of user `pi`
    - `1 Change User Password`
  - Networking:
    - `2 Network Options`
    - WIFI: `N2 Wi-fi`
    - Optional Hostname: `N1 Hostname`
  - enable SSH:
    - `5 Interfacing Options`
	- `P2 SSH`
  - Exit - with reboot
- Get IP
  - If your WIFI configuration is OK, the IP got by DHCP is printed some lines before the logon prompt.
  - If you don't see an IP on the boot screen, logon with `pi / raspberry` and type `sudo ip addr list`. The IP should be listed at the interface `wlan0`.
  - If you do not see an IP at `wlan0`, do the configuration of the WIFI again :)
- SSH to your PI (from Windows - Putty, or from Linux - ssh). Example below shows a Linux SSH connection. Insert your IP instead of `10.10.10.10`
  - `ssh pi@10.10.10.10`
  - Password: Password set before or `raspberry`
- Latest Patches / Install GIT / Reboot
  - `sudo apt update && sudo apt upgrade -y; sudo apt install -y git; sudo reboot`
- SSH to PI again
- Clone this repository
  - `mkdir ~/sw; cd ~/sw; git clone https://github.com/snorre-k/musicbox.git; cd`
- Start the installation
  - `~/sw/musicbox/scripts/start_install.sh`
  - PI gets rebootet, after installation has finished
  - You should hear a starting sound after the boot has finished

## What can I do after installation?

- Use your box as Spotify device
- Connect to your box with Bluetooth and play music
- Use your box as Plex Audio Player
- Go to http://ip-of-your-device and use __iris__ as webclient to play
  - Local Media
  - Spotify
  - TuneIn - web radio
  - local or remore radio streams
- If you have a client with Zeroconf support (AVAHI / mDNS / Bonjour), you can use http://hostname-of-pi.local
  - New Windows 10 clients support mDNS out of the box
  - Older Windows clients can install Apple [iTunes](https://support.apple.com/downloads/itunes) or [Bonjour Print Services for Windows](https://support.apple.com/kb/DL999) to get Bonjour support
- Play Music using UPnP / DLNA

## Single components installation

Single components can be installed by changing to the relevant subdirectory and starting `./install.sh`

## Additional PI config

- NTP - use DHCP supplied NTP servers: `~/sw/musicbox/scripts/various/ntp_dhcp.sh`
- VIM installation including some configuration: `~/sw/musicbox/scripts/various/vim.sh`
- Bash improvements: `~/sw/musicbox/scripts/various/better_bash.sh`
  - Colorful directory listings and grep output - `ls` shortcuts (`ll`, `l`, `la`)
  - Putty Window Title - also for root
- GPIO Shutdown/Startup on GPIO #3 and GND - OS status on definable GPIO: `~/sw/musicbox/scripts/various/GPIO.sh`
  - example [schematic](../../wiki/LED-OS-status-on-GPIO) for LED status
- Automatic Upgrades: `~/sw/musicbox/scripts/various/auto_upgrade.sh`
- Add 2nd WIFI network configuration: `~/sw/musicbox/scripts/various/add_second_known_hotspot.sh`
  - You can add the Tethering / Personal Hotspot of your smartphone to access the musicbox with the phone. This helps a lot, when the primary WIFI is not found or available.
  - Access the BOX with http://hostname-of-pi.local on your phone

## Warning

- When Plexamp (2.0) is used on a Raspberry with weak CPU, the playback stutters. Preferably don't use this on a PI with only one core. Hope this is resolved when Plexamp 2.0 leaves beta stage.
- The scripts do have only minimal error handling. If something goes wrong, most of the time the scripts do not try to solve this or stop.
