#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=u-boot
PKG_VERSION:=2013.07
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=ftp://ftp.denx.de/pub/u-boot
PKG_MD5SUM:=8445162690052e6afd4b8f87af2bb557

include $(INCLUDE_DIR)/package.mk

define uboot/Default
  TITLE:=
  CONFIG:=
  IMAGE:=
endef

define uboot/wandboard_solo
  TITLE:=U-Boot for the Wandboard Solo
endef

define uboot/wandboard_dl
  TITLE:=U-Boot for the Wandboard Dual Lite
endef

define uboot/wandboard_quad
  TITLE:=U-Boot for the Wandboard Quad
endef

UBOOTS := \
	wandboard_solo \
	wandboard_dl \
	wandboard_quad

define Package/uboot/template
define Package/uboot-imx6-$(1)
  SECTION:=boot
  CATEGORY:=Boot Loaders
  DEPENDS:=@TARGET_imx6
  TITLE:=$(2)
  URL:=http://www.denx.de/wiki/U-Boot
  VARIANT:=$(1)
  MAINTAINER:=Luka Perkov <luka@openwrt.org>
endef
endef

define BuildUBootPackage
	$(eval $(uboot/Default))
	$(eval $(uboot/$(1)))
	$(call Package/uboot/template,$(1),$(TITLE))
endef

ifdef BUILD_VARIANT
$(eval $(call uboot/$(BUILD_VARIANT)))
UBOOT_CONFIG:=$(if $(CONFIG),$(CONFIG),$(BUILD_VARIANT))
UBOOT_IMAGE:=$(if $(IMAGE),$(IMAGE),openwrt-$(BOARD)-$(BUILD_VARIANT)-u-boot.bin)
endif

define Build/Configure
	$(MAKE) -C $(PKG_BUILD_DIR) \
		$(UBOOT_CONFIG)_config
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CROSS_COMPILE=$(TARGET_CROSS)
endef

define Package/uboot/install/default
	$(INSTALL_DIR) $(BIN_DIR)/uboot-$(BOARD)-$(1)
	$(CP) \
		$(PKG_BUILD_DIR)/u-boot.imx \
		$(BIN_DIR)/uboot-$(BOARD)-$(1)/openwrt-$(BOARD)-$(1)-u-boot.imx
endef

define Package/uboot/install/template
define Package/uboot-imx6-$(1)/install
	$(call Package/uboot/install/default,$(2))
endef
endef

$(foreach u,$(UBOOTS), \
	$(eval $(call Package/uboot/install/template,$(u),$(u))) \
)

$(foreach u,$(UBOOTS), \
	$(eval $(call BuildUBootPackage,$(u))) \
	$(eval $(call BuildPackage,uboot-imx6-$(u))) \
)
