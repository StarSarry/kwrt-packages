# SPDX-License-Identifier: GPL-3.0-only
#
# Copyright (C) 2021 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=cloudreve
PKG_VERSION:=4.2.0
PKG_RELEASE:=31

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/cloudreve/Cloudreve.git
PKG_SOURCE_VERSION:=b13490357bc3ea3a570c9a68a8fb74d3865e7a2d
PKG_MIRROR_HASH:=skip

PKG_LICENSE:=GPL-3.0-only
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Tianling Shen <cnsztl@immortalwrt.org>

PKG_BUILD_DEPENDS:=golang/host node/host node-yarn/host
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=github.com/cloudreve/Cloudreve/v4
GO_PKG_LDFLAGS_X:= \
	$(GO_PKG)/pkg/conf.BackendVersion=$(PKG_VERSION) \
	$(GO_PKG)/pkg/conf.LastCommit=$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/cloudreve
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Cloud Manager
  TITLE:=A project helps you build your own cloud in minutes
  URL:=https://cloudreve.org
  DEPENDS:=@(aarch64||arm||i386||i686||riscv64||x86_64) +ca-bundle
endef

define Package/cloudreve/description
  Self-deployed file management and sharing system, supports multiple
  storage providers.
endef

define Build/Compile
	( \
		pushd $(PKG_BUILD_DIR)/assets ; \
		yarn install ; \
		yarn run build ; \
		cd .. ; \
		zip -r - assets/build > assets.zip ; \
		mv assets.zip application/statics ; \
		popd ; \
		$(call GoPackage/Build/Compile) ; \
	)
endef

define Package/cloudreve/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/Cloudreve $(1)/usr/bin/cloudreve
endef

$(eval $(call GoBinPackage,cloudreve))
$(eval $(call BuildPackage,cloudreve))
