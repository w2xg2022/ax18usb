include $(TOPDIR)/rules.mk

PKG_NAME:=qca-ssdk-shell
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2021-01-27
PKG_SOURCE_URL:=https://git.codelinaro.org/clo/qsdk/oss/ssdk-shell
PKG_SOURCE_VERSION:=5661366d471a78314bc7010f985ad8cc15be832a
PKG_MIRROR_HASH:=73111e09e896f0abbe3ee1c358aea7ec14fe5e668ce8753b8968e03c78f9599b

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define Package/qca-ssdk-shell
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Shell application for QCA SSDK
  DEPENDS:=@TARGET_ipq60xx
endef

define Package/qca-ssdk-shell/Description
  This package contains a qca-ssdk shell application for QCA chipset
endef

ifndef CONFIG_TOOLCHAIN_BIN_PATH
CONFIG_TOOLCHAIN_BIN_PATH=$(TOOLCHAIN_DIR)/bin
endif

QCASSDK_CONFIG_OPTS+= \
	ARCH=$(LINUX_KARCH) \
	KVER=$(LINUX_VERSION) \
	SYS_PATH=$(LINUX_DIR) \
	CFLAGS="$(TARGET_CFLAGS)" \
	LDFLAGS="$(TARGET_LDFLAGS)" \
	TOOLPREFIX=$(TARGET_CROSS) \
	TOOL_PATH=$(CONFIG_TOOLCHAIN_BIN_PATH)

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) $(strip $(QCASSDK_CONFIG_OPTS))
endef

define Package/qca-ssdk-shell/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/build/bin/ssdk_sh $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,qca-ssdk-shell))
