# Copyright (C) 2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=ltq-vdsl-vr9
PKG_VERSION:=4.11.4
PKG_RELEASE:=1

PKG_BASE_NAME:=drv_dsl_cpe_api_vrx
PKG_SOURCE:=$(PKG_BASE_NAME)-$(PKG_VERSION).tar.gz
PKG_BUILD_DIR:=$(KERNEL_BUILD_DIR)/drv_dsl_cpe_api-$(PKG_VERSION)
PKG_SOURCE_URL:=http://mirror2.openwrt.org/sources/
PKG_MD5SUM:=b6d9c1e3c5db1bfcd6e81bb2f582cadb

PKG_MAINTAINER:=John Crispin <blogic@openwrt.org>

include $(INCLUDE_DIR)/package.mk

define KernelPackage/ltq-vdsl-vr9
  TITLE:=vdsl driver
  SECTION:=sys
  SUBMENU:=Network Devices
  DEPENDS:=@TARGET_lantiq_xway +kmod-ltq-vdsl-vr9-mei
  FILES:=$(PKG_BUILD_DIR)/src/drv_dsl_cpe_api.ko
  AUTOLOAD:=$(call AutoLoad,51,drv_dsl_cpe_api)
endef

define Package/ltq-vdsl-vr9/description
	This package contains the Lantiq DSL CPE API driver.

	Supported Devices:
		- VRX200 Family
endef

EXTRA_CFLAGS = -fno-pic -mno-abicalls -mlong-calls -G 0

CONFIGURE_ARGS += --enable-kernel-include="$(LINUX_DIR)/include" \
	--with-max-device="1" \
	--with-lines-per-device="1" \
	--with-channels-per-line="1" \
	--enable-vrx \
	--enable-ifxos \
	--enable-ifxos-include="-I$(STAGING_DIR)/usr/include/ifxos" \
	--enable-driver-include="-I$(STAGING_DIR)/usr/include/vdsl" \
	--enable-add-drv-cflags="-DMODULE -DINCLUDE_DSL_ATM_PTM_INTERFACE_SUPPORT -DDSL_DEBUG_DISABLE" \
	--enable-adsl-led=no \
	--enable-adsl-mib=no \
	--enable-dsl-ceoc=no \
	--enable-dsl-bonding=no \
	--enable-linux-26 \
	--enable-kernelbuild="$(LINUX_DIR)" \
	KERNEL_ARCH=mips

CONFIGURE_ARGS += --enable-model=full
#CONFIGURE_ARGS += --enable-model=lite
#CONFIGURE_ARGS += --enable-model=footprint
#CONFIGURE_ARGS += --enable-model=typical
#CONFIGURE_ARGS += --enable-model=debug

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/drv_vdsl_cpe_api
	$(CP) $(PKG_BUILD_DIR)/src/include/drv_dsl_cpe*.h $(1)/usr/include/drv_vdsl_cpe_api/
endef

$(eval $(call KernelPackage,ltq-vdsl-vr9))
