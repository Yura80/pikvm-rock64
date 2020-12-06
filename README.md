# [PI-KVM](https://github.com/pikvm/pikvm) fork for Rock64, NanoPi Neo, Orange Pi and other boards

This is experimental and unsupported.

Some pre-built images are available here: https://mega.nz/folder/8c8FgYxb#zEIpTfSWMI0AVJzRoH7sbg
To make OTG keyboard/mouse work on Rock64 and Libretech boards you should use USB A-to-A cable connected to the top left USB port.

### Building

Generate a GPG key-pair if you don't have one already and export it to a public keyserver: 

    gpg --full-generate-key
    gpg --keyserver keyserver.ubuntu.com --send-key XXXXXXXXXXXXXXXX


Clone the repositories:

	git clone https://github.com/Yura80/pikvm-rock64.git
	git clone https://github.com/Yura80/os.git
	git clone https://github.com/Yura80/packages.git
	
Build the packages:
    
    cd packages
    make buildenv BOARD=generic ARCH=aarch64 _REPO_KEY=XXXXXXXXXXXXXXXX _PIBUILDER_REPO=https://github.com/Yura80/pi-builder
    make update BOARD=generic ARCH=aarch64
    make packages-generic BOARD=generic ARCH=aarch64 _REPO_KEY=XXXXXXXXXXXXXXXX _PIBUILDER_REPO=https://github.com/Yura80/pi-builder
    make buildenv BOARD=generic ARCH=arm _REPO_KEY=XXXXXXXXXXXXXXXX _PIBUILDER_REPO=https://github.com/Yura80/pi-builder
    make update BOARD=generic ARCH=arm
    make packages-generic BOARD=generic ARCH=arm _REPO_KEY=XXXXXXXXXXXXXXXX _PIBUILDER_REPO=https://github.com/Yura80/pi-builder


Upload the repository to your web server:

	make upload _REPO_DEST=root@example.com:/var/www/pikvm/
	
Copy the config template for your board:

	cd ../os
	cp ../pikvm-rock64/config.mk.rock64 config.mk
	
Edit config.mk and set PIKVM_REPO_URL and PIKVM_REPO_KEY to the proper values for the repository on your server.

Build the image:

	make os
	make image
