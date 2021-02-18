#
# Copyright (C) 2007-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libfins
PKG_VERSION:=1.0.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/petrov-adg/libfins/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=a1b3c46809ac69714a786795be976cba7f7cab7dea345ba4297773b5d62cb41e

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
#PKG_CPE_ID:=cpe:/a:htop:htop

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1
CMAKE_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libfins
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=fins
  URL:=https://github.com/petrov-adg/libfins
  DEPENDS:=+librt +zlib +libstdcpp +libpcre16 +libpthread +libatomic @!LINUX_2_6
  MAINTAINER:=Yaroslav Petrov <info@lank.me>
endef

define Package/libfins/description
 Libfins is a multi platform library with routines to communicate with Omron PLCs
 over the FINS protocol. FINS is an inter-PLC protocol which allows hardware layer
 independent exchange of data between PLCs. Omron PLCs support the exchange of data
 over serial lines, TCP, UDP and bus systems. This library currently implements the
 FINS/TCP communication.
endef

define Build/Prepare
	$(call Build/Prepare/Default)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/fins.h $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libfins.{a,so*} $(1)/usr/lib/
endef

define Package/libfins/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/libfins*.so* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libfins))