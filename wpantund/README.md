<h3> OpenThread Wpantund </h3>

This folder provides the wpantund package for OpenWrt.
https://github.com/openthread/wpantund

<h4> Install </h4>

- Add this feed to your feeds.conf
- Update/install feed
- Enable package "openthread-wpantund" in make menuconfig

<h4> Dependencies </h4>

Currently Openwrt 19.07 provides boost v1.71, which does not work out of the box together with wpantund. 
Workaround:

- Update and install your feeds
- Use the older v1.68 from OpenWrt 18.06. https://github.com/openwrt/packages/tree/openwrt-18.06/libs/boost
- Erase /feeds/packages/libs/boost/* and copy the makefile and folder incl. patches.
- Erase the /tmp folder and run "make menuconfig" again, otherwise the change will not work.

<h4> Configuration </h4>

- Edit "/etc/wpantund.conf" to modify the settings.
- Execute "wpantund" on target platform for start.
- If wpantund is running "wpanctl" is also executable.