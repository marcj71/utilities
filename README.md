# utilities
*tools and scripts to make measurements easier*

An oscilloscope is an indispensible piece of equipment. It really proves its value if you can easily capture screenshots and put them in reports or on the web. Modern digitizing oscilloscopes often have one or more interfaces to do this.
This repository has several scripts you can use to automate the process, so you don't have to press any buttons, mess with USB thumb drives. In addition, the scripts will put a timestamp on the PNG images and allow you to add a name.

The scripts all work the same: call the script from the command line, optionally followed by a description (without spaces). Upon success, they will print the name of the resulting PNG image to stdout, ready for cutting and pasting.

You will have modify each to work with your oscilloscope, as mentioned below.

## rigolscreen
This Bash script works with Ethernet-connected Rigol oscilloscopes. Modify the IP address in the script. The oscilloscope usually reports its IP address after powerup.

Uses <tt>netcat</tt>, <tt>tail</tt>, <head></tt> and ImageMagick <tt>convert</tt>.

Note that this script works with Rigol spectrum analyzers as well.

## tekscreen
This Bash script works with Ethernet-connected Tektronix oscilloscopes. Modify the IP address in the script. The oscilloscope usually reports its IP address after powerup.

Uses <tt>wget</tt> to download the screen image from the built-in web server. Again, the advantage w.r.t. to the click-and-save method is that the file is timestamped and named immediately.

## tekusbscreen
This Python 3 script downloads a screen capture from a USB-connected Tektronix oscilloscope, such as a member of the MDO3000 family. Setting it up can be a bit more tricky than the other utilities. These are dependencies for Linux:
1. Install [PyVISA](https://pyvisa.readthedocs.io/en/latest/) (<tt>sudo apt install python3-pyvisa</tt>)
1. Install [PyVISA-py](https://pyvisa-py.readthedocs.io/en/latest/) (<tt>sudo apt install python3-pyvisa-py</tt> or <tt>pip3 install pyvisa-py</tt>. Note that you don't need the [NI VISA](http://www.ni.com/download/ni-linux-device-drivers-2018/7664/en/) libraries if you use this package.
1. Install [PyUSB](https://github.com/pyusb/pyusb) (<tt>pip3 install pyusb</tt>)
1. Uncomment the line rm.listresources() to find out how your device should be addressed. If you get an error saying that the serial number cannot be read ('Found a device whose serial number cannot be read. The partial VISA resource name is...'), look at [this instruction](https://techoverflow.net/2019/08/09/how-to-fix-pyvisa-found-a-device-whose-serial-number-cannot-be-read-the-partial-visa-resource-name-is-usb0-0instr/). Here's what I did, since I was already in the <tt>adm</tt> group and figured that's rooty enough:
   1. Created /etc/udev/rules.d/50-usbgroup.rules with only this line in it:
      <tt>SUBSYSTEM=="usb", MODE="0666", GROUP="adm"</tt>
   1. sudo udevadm control --reload
   1. sudo udevadm trigger
