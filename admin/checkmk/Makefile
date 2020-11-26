#
# Copyright (C) 2007-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=checkmk
PKG_VERSION:=1.6.0p19
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/tribe29/checkmk/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=1bcf20c4f1938c563e92121932a0c9769d3b5e98eafd94343bee59f572bf2f91

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING
PKG_CPE_ID:=cpe:/a:checkmk:checkmk

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/checkmk/Default
  SECTION:=admin
  CATEGORY:=Administration
  SUBMENU:=checkmk
  TITLE:=checkmk
  URL:=https://github.com/tribe29/checkmk
  MAINTAINER:=Yaroslav Petrov <info@lank.me>
endef

define Package/checkmk-agent
  $(call Package/checkmk/Default)
  TITLE+= agent
  DEPENDS:=+xinetd
endef

define Package/checkmk-agent-plugins
  $(call Package/checkmk/Default)
  TITLE+= agent-plugins
  DEPENDS:=+checkmk-agent
endef

define Package/checkmk-agent/description
 checkmk is open source IT monitoring solution
endef

define Package/checkmk-agent-plugins/description
 checkmk-agent plugins: mysql, iptables, netstat
endef


define Build/Compile
	mkdir -p $(PKG_INSTALL_DIR)/etc/check_mk/
	$(CP) ./files/mysql.cfg $(PKG_INSTALL_DIR)/etc/check_mk/
	cd $(PKG_BUILD_DIR)/agents; rm waitmax; $(TARGET_CROSS)gcc -s -o waitmax waitmax.c; \
	mkdir -p $(PKG_INSTALL_DIR)/usr/bin/; \
	cp $(PKG_BUILD_DIR)/agents/check_mk_agent.linux $(PKG_INSTALL_DIR)/usr/bin/check_mk_agent; \
	cp $(PKG_BUILD_DIR)/agents/waitmax  $(PKG_INSTALL_DIR)/usr/bin/waitmax; \
	cp $(PKG_BUILD_DIR)/agents/mk-job  $(PKG_INSTALL_DIR)/usr/bin/mk-job; \
	mkdir -p $(PKG_INSTALL_DIR)/etc/xinetd.d/; \
	cp $(PKG_BUILD_DIR)/agents/cfg_examples/xinetd.conf $(PKG_INSTALL_DIR)/etc/xinetd.d/check-mk-agent
	mkdir -p $(PKG_INSTALL_DIR)/usr/lib/check_mk_agent/plugins/; \
	cp $(PKG_BUILD_DIR)/agents/plugins/mk_iptables $(PKG_INSTALL_DIR)/usr/lib/check_mk_agent/plugins/; \
	cp $(PKG_BUILD_DIR)/agents/plugins/mk_mysql $(PKG_INSTALL_DIR)/usr/lib/check_mk_agent/plugins/; \
	cp $(PKG_BUILD_DIR)/agents/plugins/netstat.linux $(PKG_INSTALL_DIR)/usr/lib/check_mk_agent/plugins/netstat
endef

define Package/checkmk-agent/install
	$(INSTALL_DIR) $(1)/etc/xinetd.d/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/etc/xinetd.d/check-mk-agent $(1)/etc/xinetd.d/
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/check_mk_agent $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/waitmax $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/mk-job $(1)/usr/bin/
endef

define Package/checkmk-agent-plugins/install
	$(INSTALL_DIR) $(1)/etc/check_mk/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/etc/check_mk/mysql.cfg $(1)/etc/check_mk/
	$(INSTALL_DIR) $(1)/usr/lib/check_mk_agent/plugins/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/check_mk_agent/plugins/mk_iptables $(1)/usr/lib/check_mk_agent/plugins/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/check_mk_agent/plugins/mk_mysql $(1)/usr/lib/check_mk_agent/plugins/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/check_mk_agent/plugins/netstat $(1)/usr/lib/check_mk_agent/plugins/
endef

$(eval $(call BuildPackage,checkmk-agent))
$(eval $(call BuildPackage,checkmk-agent-plugins))
