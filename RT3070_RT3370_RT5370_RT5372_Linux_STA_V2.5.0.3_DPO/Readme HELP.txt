################# How to build SDK to use USB wireless###############################
# You should do steps following:
#	1. Staying csrootf package. 
#			make menuconfig
#		=> Make sure "[*]   wireless tool" is checked. If it's ready, you can see this command "iwpriv"in <your csrootf>/build_mipsel/root/sbin/.
#	2. Copy file RT2870STA.dat to <your csrootf>/build_mipsel/root/etc/Wireless/RT2870STA/
#	3. Re-build your csrootf
#	4. Re-build your kernel 
#	5. Copy vmlinux.bin from your kernel to your tftpboot
#	6. Open file: 2011_0719_RT3070_RT3370_RT5370_RT5372_Linux_STA_V2.5.0.3_DPO/Makefile
#		- Line 15: make sure It don't have "#" before PLATFORM = SIGMA 
#		- Line 43, 44, 45, 46, 47:
#			+ LINUX_SRC =<the path to your kernel>/<linux>
#				EX: 	LINUX_SRC = /working/smp86xx_kernel_source_R2.6.22-24/linux-2.6.22.19/
#			+ CROSS_COMPILE = <the path to your csrootf>/host/bin/mipsel-linux-
			+ Line 326 ->346 edit <path>/working/kenvi/huyle/Code/RT3070_RT3370_RT5370_RT5372_Linux_STA_V2.5.0.3_DPO/output
#	   Staying 2009_0525_RT3070_Linux_STA_v2.1.1.0
source SDK
cd /working/kenvi/huyle/Code/RT3070_RT3370_RT5370_RT5372_Linux_STA_V2.5.0.3_DPO
mkdir output
#			make clean;make
#	7. Finish all 6 steps above, you have the driver with name: rt3070sta.ko in 2009_0525_RT3070_Linux_STA_v2.1.1.0/os/linux/
######################################################################################################

###### How to test wireless connect tion  with cmd #################################

#Insmod driver
insmod ./rt3070sta.ko

modprobe usb-storage
modprobe tangox-ehci-hcd
modprobe tangox-ohci-hcd
modprobe sata_tango3

ifconfig ra0 inet <your ip(make sure It's not conflict with another one)> up

### Some following commands to test wireless in your place.

iwpriv ra0 set AuthMode		=	<your wireless Authen. Mode>
iwpriv ra0 set EncrypType	=	<your wireless EnCryp Type>
iwpriv ra0 set SSID			=	"<your wireless SSID>"
iwpriv ra0 set DefaultKeyID	=	<your wireless default key>
iwpriv ra0 set Key1			=	"<your wireless password>"

route add default gw <your default gateway>

#Test wireless connection (you will see all available wireless around)
iwlist ra0 scanning
iwpriv ra0 get_site_survey

iwpriv ra0 set NetworkType	= Infra
iwpriv ra0 set AuthMode		= WPA2PSK
iwpriv ra0 set EncrypType	= TKIP
iwpriv ra0 set DefaultKeyID	= 1
iwpriv ra0 set SSID			= Sigma_MyTVgroup
iwpriv ra0 set Key1			= SigmaSigmaMYTV

iwpriv ra0 set NetworkType=Infra
iwpriv ra0 set AuthMode=WPAPSK
iwpriv ra0 set EncrypType=TKIP
iwpriv ra0 set SSID="Sigma_MyTVgroup"
iwpriv ra0 set WPAPSK="SigmaSigmaMYTV"

iwpriv ra0 set SSID="AP's SSID"


TKIPAES