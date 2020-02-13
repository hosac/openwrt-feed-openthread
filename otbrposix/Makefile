# https://github.com/hosac | hosac@gmx.net

include $(TOPDIR)/rules.mk

PKG_NAME:=openthread-otbrposix
PKG_VERSION:=2020-01-22
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/openthread/ot-br-posix.git
PKG_SOURCE_VERSION:=95ac6cccdd0dbbd4356fa19dbec1cf4cf3d1221d

include $(INCLUDE_DIR)/package.mk

define Package/openthread-otbrposix
	SECTION:=base
	CATEGORY:=Network
	TITLE:=OpenThread border router
	DEPENDS:=+libjson-c +libubox +libubus +libblobmsg-json +avahi-dbus-daemon +libstdcpp +libreadline +kmod-usb-acm +kmod-tun +kmod-ieee802154_6lowpan +kmod-mac802154
endef

define Package/openthread-otbrposix/description
	A mesh network border router implementation -application.
endef

define Build/Configure
	@echo "--- This is the host plattform:   " $(GNU_HOST_NAME)
	@echo "--- This is the target plattform: " $(GNU_TARGET_NAME)
	
	@echo "--- Build/Configure ---"
	cd $(PKG_BUILD_DIR) && ./bootstrap
	cd $(PKG_BUILD_DIR) && ./configure \
		--target=$(GNU_TARGET_NAME) \
		--host=$(GNU_TARGET_NAME) \
		--enable-openwrt \
		--with-ncp=openthread \
		--prefix= --exec-prefix=/usr \
		--disable-docs \
		--disable-web-service \
		--with-mdns=none \

	# copy dbus files to avoid build errors
	$(INSTALL_DIR) $(PKG_BUILD_DIR)/src/dbus
	$(CP) $(BUILD_DIR)/dbus-*/dbus/dbus* $(PKG_BUILD_DIR)/src/dbus
	# copy ubox/ubus files to avoid build errors
	$(INSTALL_DIR) $(PKG_BUILD_DIR)/src
	$(CP) $(STAGING_DIR)/usr/include/libubox $(PKG_BUILD_DIR)/src
	$(CP) $(STAGING_DIR)/usr/include/libubus.h $(PKG_BUILD_DIR)/src
	$(CP) $(STAGING_DIR)/usr/include/ubusmsg.h $(PKG_BUILD_DIR)/src
	$(CP) $(STAGING_DIR)/usr/include/ubus_common.h $(PKG_BUILD_DIR)/src
	# copy avahi files to avoid build errors
	$(INSTALL_DIR) $(PKG_BUILD_DIR)/src
	$(CP) $(STAGING_DIR)/usr/include/avahi-client $(PKG_BUILD_DIR)/src
	$(INSTALL_DIR) $(PKG_BUILD_DIR)/src
	$(CP) $(STAGING_DIR)/usr/include/avahi-common $(PKG_BUILD_DIR)/src

endef

define Build/Compile
	@echo "> --- Build/Compile ---"
	@echo $(STAGING_DIR)
	@echo $(PKG_BUILD_DIR)
	cd $(PKG_BUILD_DIR) && HOST=$(GNU_TARGET_NAME) TARGET=$(GNU_TARGET_NAME) $(MAKE)
endef

define Package/openthread-otbrposix/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/controller/admin
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/view
	$(INSTALL_DIR) $(1)/www/luci-static/resources
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/agent/otbr-agent $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/openwrt/controller/thread.lua $(1)/usr/lib/lua/luci/controller/admin
	$(CP) $(PKG_BUILD_DIR)/src/openwrt/view/admin_thread $(1)/usr/lib/lua/luci/view
	$(CP) $(PKG_BUILD_DIR)/src/openwrt/handle_error.js $(1)/www/luci-static/resources
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/openwrt/otbr $(1)/etc/init.d
endef

$(eval $(call BuildPackage,openthread-otbrposix))
