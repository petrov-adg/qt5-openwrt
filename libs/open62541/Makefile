#
# Copyright (C) 2007-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=open62541
PKG_VERSION:=1.1.2
PKG_RELEASE:=1

PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/open62541/open62541/archive/
PKG_HASH:=29c809334cb28af617d6526b1602b6df07cc33f36fce32f6fc913f8e83564cca

PKG_LICENSE:=MPLv2
PKG_LICENSE_FILES:=LICENSE
#PKG_CPE_ID:=cpe:/a:htop:htop

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1
CMAKE_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libopen62541
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=open62541
  URL:=https://open62541.org/
  DEPENDS:=+librt +zlib +libstdcpp +libpcre16 +libpthread +libatomic @!LINUX_2_6
  MAINTAINER:=Yaroslav Petrov <info@lank.me>
endef

define Package/libopen62541/description
 open62541 (http://open62541.org) is an open source and free implementation of
 OPC UA (OPC Unified Architecture) written in the common subset of the C99 and
 C++98 languages. The library is usable with all major compilers and provides
 the necessary tools to implement dedicated OPC UA clients and servers, or to
 integrate OPC UA-based communication into existing applications. open62541
 library is platform independent.
endef

CMAKE_OPTIONS += \
        -DBUILD_SHARED_LIBS=ON \
        -DUA_NAMESPACE_ZERO=FULL \
        -DUA_ENABLE_AMALGAMATION=OFF

define Build/Prepare
	$(call Build/Prepare/Default)
	(cd $(PKG_BUILD_DIR); cd deps/; rm -fr ua-nodeset; git clone https://github.com/OPCFoundation/UA-Nodeset.git ua-nodeset)
endef

define Package/libopen62541/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/libopen62541.so* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libopen62541))
