include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=tcp-brutal
PKG_VERSION:=1.0.3
PKG_RELEASE:=1

PKG_URL:=https://github.com/apernet/tcp-brutal
PKG_SOURCE_URL:=$(PKG_URL).git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_DATE:=2024-07-30
PKG_MIRROR_HASH:=0c7f5581da3bc5726bfd36a1f4863f77ca9a2684449d4b1d416577557b3d6f92
PKG_LICENSE:=GPL-3.0

PKG_MAINTAINER:=Haruue Icymoon <i@haruue.moe>

include $(INCLUDE_DIR)/package.mk

define KernelPackage/brutal
	SECTION:=kernel
	SUBMENU:=Network Support
	TITLE:=TCP Brutal Kernel module
	FILES:=$(PKG_BUILD_DIR)/brutal.$(LINUX_KMOD_SUFFIX)
endef

define KernelPackage/brutal/description
	Hysteria's congestion control algorithm ported to kernel mode TCP
endef

include $(INCLUDE_DIR)/kernel-defaults.mk

define Build/Configure
endef

define Build/Compile
	$(KERNEL_MAKE) M="$(PKG_BUILD_DIR)" modules
endef

$(eval $(call KernelPackage,brutal))
