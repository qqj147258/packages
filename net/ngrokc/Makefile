#
# Copyright (C) 2006-2015 OpenWrt.org
# Copyright (C) 2016 LEDE Project
# Copyright (C) 2016 ibrother <i@ibrother.me>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.

include $(TOPDIR)/rules.mk

PKG_NAME:=ngrokc
PKG_VERSION:=1.37
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/dosgo/ngrok-c.git
PKG_SOURCE_VERSION:=fb69f23943e9f5bdfda479cc41a08e6d4656b7ac
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_RELEASE)

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=ibrother <i@ibrother.me>

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)/$(PKG_SOURCE_SUBDIR)

PKG_USE_MIPS16:=0
PKG_BUILD_PARALLEL:=1

BUILD_ARGS += \
		-Wall \
		-fexceptions \
		-DOPENSSL=1 \
		-DOPENSSLDL=1 \

CC:=mips-openwrt-linux-gcc
CXX:=mips-openwrt-linux-g++

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	TITLE:=ngrok client
	DEPENDS:=+libopenssl +libstdcpp
	URL:=https://github.com/dosgo/ngrok-c
endef

define Package/$(PKG_NAME)/description
ngrok client
endef

define Package/$(PKG_NAME)/conffiles
/etc/config/ngrokc
endef

define Build/Compile
	$(CXX) $(BUILD_ARGS) -c $(PKG_BUILD_DIR)/sendmsg.cpp -o $(PKG_BUILD_DIR)/sendmsg.o
	$(CXX) $(BUILD_ARGS) -c $(PKG_BUILD_DIR)/openssldl.cpp -o $(PKG_BUILD_DIR)/openssldl.o
	$(CC) $(BUILD_ARGS) -c $(PKG_BUILD_DIR)/cJSON.c -o $(PKG_BUILD_DIR)/cJSON.o
	$(CXX) $(BUILD_ARGS) -c $(PKG_BUILD_DIR)/main.cpp -o $(PKG_BUILD_DIR)/main.o
	$(CXX) $(BUILD_ARGS) -c $(PKG_BUILD_DIR)/nonblocking.cpp -o $(PKG_BUILD_DIR)/nonblocking.o
	$(CXX) $(BUILD_ARGS) -c $(PKG_BUILD_DIR)/sslbio.cpp -o $(PKG_BUILD_DIR)/sslbio.o
	$(CXX) $(BUILD_ARGS) -c $(PKG_BUILD_DIR)/ngrok.cpp -o $(PKG_BUILD_DIR)/ngrok.o
	$(CXX) -s $(PKG_BUILD_DIR)/main.o $(PKG_BUILD_DIR)/cJSON.o $(PKG_BUILD_DIR)/sendmsg.o $(PKG_BUILD_DIR)/nonblocking.o $(PKG_BUILD_DIR)/ngrok.o $(PKG_BUILD_DIR)/sslbio.o $(PKG_BUILD_DIR)/openssldl.o -o $(PKG_BUILD_DIR)/ngrokc
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ngrokc $(1)/usr/bin/ngrokc
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/ngrokc.config $(1)/etc/config/ngrokc
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/ngrokc.init $(1)/etc/init.d/ngrokc
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
