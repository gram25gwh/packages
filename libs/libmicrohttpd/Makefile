#
# Copyright (C) 2010-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libmicrohttpd
PKG_VERSION:=0.9.68
PKG_RELEASE:=1
PKG_MAINTAINER:=Alexander Couzens <lynxis@fe80.eu>
PKG_LICENSE:=LGPL-2.1
PKG_LICENSE_FILES:=COPYING

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@GNU/libmicrohttpd
PKG_HASH:=c5716e2a2899abc9e16c8fa7a1a58da88f81aca96a0b7e68a5d4d89e21610b61

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libmicrohttpd/default
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=GNU libmicrohttpd is a library that runs an HTTP server.
  URL:=http://www.gnu.org/software/libmicrohttpd/
endef

define Package/libmicrohttpd-no-ssl
$(call Package/libmicrohttpd/default)
  VARIANT:=nossl
  DEFAULT_VARIANT:=1
  PROVIDES:=libmicrohttpd
endef

define Package/libmicrohttpd-ssl
$(call Package/libmicrohttpd/default)
  TITLE+= with SSL support
  VARIANT:=ssl
  DEPENDS:=+libgcrypt +libgnutls +libgpg-error
  PROVIDES:=libmicrohttpd
endef

CONFIGURE_ARGS+= \
	--disable-curl \
	--disable-rpath \
	--disable-doc \
	--disable-examples \
	--enable-poll=no \
	--enable-epoll=yes \

ifeq ($(BUILD_VARIANT),ssl)
CONFIGURE_ARGS += \
	--enable-https \
	--with-gnutls
else
CONFIGURE_ARGS += \
	--disable-https \
	--without-gnutls
endif

define Package/libmicrohttpd/description
  GNU libmicrohttpd is a small C library that is supposed to make it easy
  to run an HTTP server as part of another application.
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/include/microhttpd.h $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libmicrohttpd.{so*,a,la} $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libmicrohttpd.pc $(1)/usr/lib/pkgconfig/
endef

define Package/libmicrohttpd-no-ssl/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libmicrohttpd.so* $(1)/usr/lib/
endef

define Package/libmicrohttpd-ssl/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libmicrohttpd.so* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libmicrohttpd-no-ssl))
$(eval $(call BuildPackage,libmicrohttpd-ssl))
