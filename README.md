sources:
1. https://davidalger.com/posts/fedora-asahi-remix-on-apple-silicon-with-luks-encryption/
2. https://github.com/osx-tools/asahi-encrypt

`curl https://alx.sh | sh`

make sure you have 60-ish GB of extra space for the second asahi linux installation, if not resize or run the script again with the expert mode parameter activated
- pick the 4th option (minimal asahi)

# booting into minimal asahi

the terminal will greet you with extremely small text! if you'd prefer not to squint, type this to increase the text size:
`setfont -d`

you will also be greeted with slightly unintelligible messages from the kernel about random processes. 
these low level messages will also print to STDOUT as you're typing commands, pushing your cursor to a newline (it's not a very user friendly experience!).
however it's fun to plug devices into the USB-C ports of your mac and see kernel messages pop up (i am a nerd though so i don't know if this is objectively fun lmao).

you won't have wifi, so use this syntax:
`nmcli dev wifi connect NETWORK_SSID password NETWORK_PASSWORD`

then install git with dnf, and git clone this:

`git clone https://github.com/osx-tools/asahi-encrypt`

use `fdisk -l` and/or `lsblk -f` to view partitions, make sure to note the correct partition for your main asahi installation.
set executable perms for the asahi-encrypt script, then first list viable unencrypted partitions with:
`./asahi-encrypt -l`

cross reference this with the partition that you're absolutely sure is your main asahi installation (`nvme0n1p6` in this example), then encrypt with:
`./asahi-encrypt /dev/nvme0n1p6`


# make sure to change the default boot before you uninstall the minimal asahi installation
*note: it might be good to keep the minimal asahi installation to use as a recovery OS*

- go into macos system settings -> general -> startup disk
- then select macintosh HD
- then delete the partition and resize it using `diskutil` (consult the official asahi docs to do this properly)
