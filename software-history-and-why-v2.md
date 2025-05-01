# Software History and why V2

* The original Sensorgnome software was written and maintained by John Brzustowski from 2016 to 2018 and was designed for Beaglebone and Raspberry Pi models 1 and 2 single board computers. It used an ingenious but very non-standard "liwixi" filesystem organization and was based on Linux Debian Buster. Communication with Motus servers used SSH tunnels in an effective but unconventional configuration.
* The Sensorgnome V2 software is a complete rewrite of the system with the goal of supporting current Raspberry Pi models and using standard filesystem and communication methods.
* The V2 software:
  * uses a standard current Raspberry Pi OS image (based on Debian Bullseye as of 2023) that has Sensorgnome software pre-installed
  * runs on Raspberry Pi3, Pi4, Zero-2W, and SensorStation V1 (more coming)
  * exclusively uses HTTPS for Internet communication (SSH commandline access over the LAN is also supported),
  * implements a new automatic upload mechanism that uploads data files directly to motus.org,
  * implements a new web UI with more functionality and security to manage the Sensorgnome,
  * the new web UI provide easy options to download data files to a laptop or phone,
  * implements remote monitoring and management,
* The V2 software uses unmodified software to process radio data and detect tags in the local tag database, thus the data processing path is unchanged.
