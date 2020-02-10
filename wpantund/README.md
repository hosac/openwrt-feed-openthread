<h3> openthread-wpantund </h3>

This folder provides the https://github.com/openthread/wpantund package. Inspired by https://github.com/NordicPlayground/thread_border_router_feeds

</br>
<h4> Dependencies </h4>

In January 2020 Openwrt 19.07 provides boost library in v1.71, which does not work out of the box together with wpantund. 
Workaround:

- Update and install your feeds
- Use the older v1.68 from OpenWrt 18.06. https://github.com/openwrt/packages/tree/openwrt-18.06/libs/boost
- Erase /feeds/packages/libs/boost/* and copy the makefile and folder incl. patches.
- Erase the /tmp folder and run "make menuconfig" again, otherwise the change will not work.

</br>
<h4> Configuration </h4>

- Edit "/etc/wpantund.conf" to modify the settings you want.

</br>
<h4> Notes </h4>

- The package does not provide autostart or init.d script - please use it manually in terminal
- Execute "wpantund" on target platform for start.
- If wpantund is running "wpanctl" is also executable.