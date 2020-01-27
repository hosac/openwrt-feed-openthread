include $(TOPDIR)/rules.mk

PKG_NAME:=openthread-wpantund
PKG_VERSION:=2020-01-15
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/openthread/wpantund.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=3c1696b1b472b9e551f4513febc5bc68b4140bda
PKG_FIXUP:=autoreconf
PKG_USE_MIPS16:=0
PKG_BUILD_DEPENDS:=autoconf-archive/host
PKG_LICENSE:=ASL-2.0

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/target.mk
include $(INCLUDE_DIR)/host-build.mk

MAKE_PATH:=src
CONFIGURE_ARGS += \
	--sysconfdir=/etc \
	--enable-debug

define Host/Configure
	$(call Host/Configure/Default)
endef

define Host/Compile
	$(call Host/Compile/Default)
endef

define Host/Install
	$(call Host/Install/Default)
endef

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	TITLE:=OpenThread Userspace WPAN Network Daemon
	DEPENDS:=+libreadline +dbus +libstdcpp +libncurses +boost +boost-signals +boost-system +kmod-usb-acm +kmod-tun +kmod-ieee802154_6lowpan +kmod-mac802154 +coreutils +coreutils-stty
endef

define Package/$(PKG_NAME)/description
	wpantund is a user-space network interface driver/daemon that provides a native IPv6 network interface to a low-power wireless Network Co-Processor (or NCP).
	It was written and developed by Nest Labs to make supporting Thread connectivity on Unix-like operating systems more straightforward.

	wpantund is designed to marshall all access to the NCP, ensuring that it always remains in a consistent and well-defined state.
endef

define Build/Configure
	$(call Build/Configure/Default,--sysconfdir=/etc)
endef

TARGET_CXXFLAGS += -Wno-error=cpp

PKG_FIXUP:=autoconf

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/
	$(INSTALL_DIR) $(1)/usr/local/bin/
	$(INSTALL_DIR) $(1)/usr/local/etc/
	$(INSTALL_DIR) $(1)/usr/local/lib/
	$(INSTALL_DIR) $(1)/usr/local/include/wpantund/
	$(INSTALL_DIR) $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/etc/dbus-1/system.d/
	$(INSTALL_DIR) $(1)/usr/local/share/man/man1/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/wpantund/wpantund $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/wpanctl/wpanctl $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/ncp-spinel/ncp-spinel.la $(1)/usr/lib/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/ncp-dummy/ncp-dummy.la $(1)/usr/lib/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/ncp-spinel/.libs/ncp-spinel.so $(1)/usr/lib/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/ncp-spinel/ncp-spinel.la $(1)/usr/local/lib/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/ncp-dummy/.libs/ncp-dummy.so $(1)/usr/local/lib/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/ncp-dummy/ncp-dummy.la $(1)/usr/local/lib/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/ipc-dbus/wpan-dbus-v0.h $(1)/usr/local/include/wpantund/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/ipc-dbus/wpan-dbus-v1.h $(1)/usr/local/include/wpantund/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/ipc-dbus/wpantund.conf $(1)/etc/dbus-1/system.d/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/wpantund/wpantund.conf $(1)/etc/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/wpantund/wpan-properties.h $(1)/usr/local/include/wpantund
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/wpantund/wpan-error.h $(1)/usr/local/include/wpantund
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/doc/wpantund.1 $(1)/usr/local/share/man/man1/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/doc/wpanctl.1 $(1)/usr/local/share/man/man1/
	$(CP) ./files/wpantund.conf $(1)/etc
	chmod 0644 $(1)/etc/wpantund.conf
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,$(PKG_NAME)))
