The openaps-overlay.dts file contains settings for a Raspbian overlay file that will configure the GPIO pins for use with OpenAPS.
Compile the dts file into a dtbo file with the following command...

dtc -W no-unit_address_vs_reg -@ -I dts -O dtb -o openaps.dtbo openaps-overlay.dts


Next copy the openaps.dtbo file to /boot/overlays/ and then edit the /boot/config.txt file and add a line to load the overlay file, i.e.

dtoverlay=openaps

