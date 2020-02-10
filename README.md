<img src="https://www.gstatic.com/devrel-devsite/prod/v172e5dffd78b32f4b12f8112b00e940d4993af48229fac5346097b33edb0f543/openthread/images/lockup.svg" width="250" title="OpenThread Image">

This repository is based on the OpenThread project https://openthread.io and https://github.com/openthread with focus on OpenWrt.
</br>
<h4> Add feeds </h4>

Edit your "feeds.conf" and add

	src-git openthread https://github.com/hosac/openwrt-feed-openthread.git

Update and install

	./scripts/feeds update openthread
	./scripts/feeds install -a -p openthread
	
</br>
<h4> Available packages for build system</h4>

(Please find Readme.md in dedicated folders)



	make menuconfig

Choose the packages you need in "Network" section
	
	> Network
		> openthread-wpantund (single application, independent from otbrposix)
		> openthread-otbrposix (single application, independent from wpantund)






