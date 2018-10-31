# gitsdreasy
SDR Easy for Raspberry PI

download gitsdreasy.doc

First download the OS needed..... 

http://vx2-downloads.raspberrypi.org/raspbian/images/raspbian-2017-08-17/2017-08-16-raspbian-stretch.zip
Full version

http://vx2-downloads.raspberrypi.org/raspbian_lite/images/raspbian_lite-2017-08-17/2017-08-16-raspbian-stretch-lite.zip
Lite version

Gear required :
SDR RTL 2832
Raspberry PI ( adafruit 1497) 
8 gig mini SD 


SDR - RTL 2832 w/ R820T .. Adafruit 1497

Not recommended at this time.. Check www.sdr-rtl.com for their neat dongles.  And look at others as well.


Flash 8 GB Mini SD card with latest Stretch image


Modify using Raspberry Pi Configuration program for US Locale, US keyboard, Time Zone, and WiFi Location.


Activate WiFi connection, or other internet connectivity ( RJ-45 )


As a time saving activity, I make a new image of my Raspberry Pi SD card at each major step.  If I get to a place that works as expected, then I make the image so I don’t have to do all that work again in case I corrupt my image somehow.  I use Win32DiskImager and Etcher for this work.


From Terminal, run

        sudo apt-get update

        sudo apt-get upgrade


From Terminal, run

        lsusb


Gives listing of what is in USB ports.. Adafruit SDR is Realtek Semiconductor RTL2838 DVB-T


Bus 001 Device 007: ID 045e:071d Microsoft Corp.

Bus 001 Device 006: ID 1b3f:2008 Generalplus Technology Inc.

Bus 001 Device 005: ID 046d:c526 Logitech, Inc. Nano Receiver

Bus 001 Device 004: ID 0bda:2838 Realtek Semiconductor Corp. RTL2838 DVB-T

Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp. SMSC9512/9514 Fast Ethernet Adapter

Bus 001 Device 002: ID 0424:9514 Standard Microsystems Corp. SMC9514 Hub

Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub


_______________________________________________________________


Installing various SDR programs


__________________________________________________

                                                                         .

RTL-SDR installation  ( MOST RECENT )


Credits to Steve Schwartz for video, Kenn Ranous for research


From https://www.youtube.com/watch?v=1pr319FvOwI

And  https://www.youtube.com/watch?v=mBU7eSviy_0&feature=youtu.be



Starting with a fresh flashed card, Stretch Distro.  Do the localization setups in Menu/Preferences/Raspberry Pi Configuration… then reboot


Then sign into the wifi ..  The Pi must be on line for this exercise…..


Modify Terminal parameters for font = 12 and foreground = #FFFFFF, 50 X 40

 ( because you’re old and can’t see well )


From Terminal, run

        sudo apt-get update

        sudo apt-get upgrade


From Terminal, cd /etc/apt   then


        sudo apt-get install git

  ( New files installed )

        

sudo apt-get install cmake

          ( New files installed )

        

sudo apt-get install build-essential

  ( in my case, nothing important happens, git already newest version )


        sudo apt-get install libusb-1.0-0-dev

  ( New files installed )


        sudo git clone git://git.osmocom.org/rtl-sdr.git

          ( New files installed )

 

        then cd rtl-sdr/


        sudo mkdir build


        cd build

        ls (to see it's empty)


        sudo cmake ../  -DINSTALL_UDEV_RULES=ON

        sudo make

         ( compiling drivers from the source )


        sudo make install

         ( Installing drivers )


        sudo ldconfig

         ( examine libraries, update links and cache )


        sudo cp ../rtl-sdr.rules /etc/udev/rules.d/   (don't miss the space before /etc)


        cd /etc  

        ls  (to see the results of all this work)


        cd modprobe.d

        ls ( to see its content - 6 files in my case )

        sudo touch blacklist-rtl.conf

        ls ( to see the blacklist-rtl.conf file )


        sudo nano blacklist-rtl.conf ( edit to add a line )


        Line to be added... blacklist dvb_usb_rtl28xxu

        Then CTRL o, Enter, CTRL x  (writes the file and saves it)


        sudo cat blacklist-rtl.conf (to see the file's contents and verify)


reboot raspberry pi, and make sure the RTL device is in the USB port


        rtl_test -t


Should return


        Found 1 device(s):

  0:  Realtek, RTL2838UHIDIR, SN: 00000001


Using device 0: Generic RTL2832U OEM

Found Rafael Micro R820T tuner

Supported gain values (29): 0.0 0.9 1.4 2.7 3.7 7.7 8.7 12.5 14.4 15.7 16.6 19.7 20.7 22.9 25.4 28.0 29.7 32.8 33.8 36.4 37.2 38.6 40.2 42.1 43.4 43.9 44.5 48.0 49.6

[R82XX] PLL not locked!

Sampling at 2048000 S/s.

No E4000 tuner found, aborting.


At this point, I used Win32DiskImager to create an image of the sd card.  Much too much work to do this over and over.  From this point, installing GQRX will be another adventure..



Now to install GQRX


from the website gqrx.dk/download/gqrx-sdr-for-the-raspberry-pi


click on the link Gqrx SDR for armv6 ( alternate download :SourceForge )   link

This downloads the latest version of GQRX ( 2.9 at this time )


Then navigate to the downloads folder on the Pi

        

        pwd ( for present working directory )

        ls  ( to see what is in that directory, and look for downloads directory )


In my case it looked like this:


pi@raspberrypi:~ $ pwd

/home/pi

pi@raspberrypi:~ $ ls

Desktop  Documents  Downloads  Music  Pictures  Public  python_games  Templates  Videos

pi@raspberrypi:~ $ cd Downloads

pi@raspberrypi:~/Downloads $


        ls ( to see the downloaded file )


In my case it looked like this:


gqrx-2.9-linux-armv6.tar.xz



        tar xf gqrx-2.9-linux-armv6.tar.xz (to extract the files)


        ls to see the new folder gqrx-2.9-linux-armv6


        cd gqrx-2.9-linux-armv6

        ls ( to see what's there )

        

        cat readme.txt


results in


This is Gqrx v2.9 for the Rapsberry Pi.


It was built on a Raspberry Pi 3 running Raspbian Stretch, but should

also work on other similar ARM boards running a Debian 9 based Linux

distribution.


You need to install the GNU Radio and Qt5 libraries:


sudo apt-get install gnuradio libvolk1-bin libusb-1.0-0 gr-iqbal

sudo apt-get install qt5-default libqt5svg5 libportaudio2



When installation done, please run the volk_profile tool as the user who

will be running gqrx.


sudo apt-get install libvolk1-bin
volk_profile



Then, to redirect the audio from the HDMI port to the headphone jack on the Pi

amixer cset numid=3 1


The bundle includes SoapySDR 0.6 but no plugins. Build your plugins and

then use the SOAPY_SDR_ROOT environment variable to tell SoapySDR where

to look for them.


Finally, to enable user access to rtlsdr, Airspy, Funcube Dongle or

HackRF you need to copy the appropriate udev rule to /etc/udev/rules.d/

before plugging in your device. For example:


sudo cp udev/rtl-sdr.rules /etc/udev/rules.d/

 (this was already done in the above sequence from Steve Schwartz  )



You might be able to reduce CPU load by running the volk_profile tool (can

be done at any time).


Please go to http://gqrx.dk/user-support if you need assistance.


From the instructions above

        sudo apt-get install gnuradio libvolk1-bin libusb-1.0-0 gr-iqbal
        sudo apt-get install qt5-default libqt5svg5 libportaudio2

sudo apt-get install libvolk1-bin
        volk_profile

______________________________________________________________

RTL-SDR  ( OBSOLETE Jan 2018  Really needs some more work  )

run

        sudo apt-get install rtl-sdr

