-------------------------------------------------------------------------------
Linux Setup 
-------------------------------------------------------------------------------

Download the required packages:

	eclipse-java-*-linux-gtk-x86_64.tar.gz
	OpenJDK11U-jdk_x64_linux_hotspot_11*.tar.gz
	commandlinetools-linux-*_latest.zip
	android.mytools-main.zip
	
Extract the setup_android_dev script from the mytools archive and run from the
same directory as the downloads. This will install the tools based on the
following layout:

$HOME/Android
			setenv
			Projects...
			
$HOME/.local/
			android-sdk
			eclipse
			jdk-11.0.19+7

Open a terminal and cd into $HOME/Android to set the environment:

	$ . setenv

Get the latest packages list:

	$ sdkmanager --list > list.txt
	
Use it to get the required build tools and platform, e.g.

	$ sdkmanager "build-tools;33.0.1" "platform-tools" "platforms;android-28"

-------------------------------------------------------------------------------
Set ADB Privileges
-------------------------------------------------------------------------------

Connect the Android device and check device vendor id and product id via dmesg.

For example:

	[47119.325757] usb 2-6.4: new high-speed USB device number 9 using ehci-pci
	[47119.434597] usb 2-6.4: New USB device found, idVendor=1949, idProduct=0338, bcdDevice= 2.23
	[47119.434600] usb 2-6.4: New USB device strings: Mfr=1, Product=2, SerialNumber=3
	[47119.434602] usb 2-6.4: Product: KFKAWI
	[47119.434604] usb 2-6.4: Manufacturer: Amazon
	....

Create a udev rule:

	$ sudo nano /etc/udev/rules.d/51-android.rules

	SUBSYSTEM=="usb", ATTR{idVendor}=="1949", ATTR{idProduct}=="0338", MODE="0666", GROUP="plugdev"

Reload the rules:

	$ sudo udevadm control --reload-rules

Reconnect the device to allow detection.

-------------------------------------------------------------------------------
