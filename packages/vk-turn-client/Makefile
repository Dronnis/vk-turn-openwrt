include $(TOPDIR)/rules.mk

PKG_NAME:=vk-turn-client
PKG_VERSION:=2.0.7
PKG_RELEASE:=1
PKG_BUILD_PARALLEL:=0

PKG_LICENSE:=MIT
PKG_MAINTAINER:=Auto-builder <auto@github>

include $(INCLUDE_DIR)/package.mk

define Package/vk-turn-client
  SECTION:=net
  CATEGORY:=Network
  TITLE:=VK TURN Proxy Client for OpenWRT
  URL:=https://github.com/kiper292/vk-turn-proxy
  DEPENDS:=+procd-ujail
endef

define Package/vk-turn-client/description
  Client component of VK TURN Proxy. Precompiled binary downloaded from upstream GitHub Releases.
endef

UPSTREAM_ARCH := $(subst x86_64,amd64,$(subst aarch64,arm64,$(subst mipsel,mipsle,$(subst i386,386,$(ARCH)))))

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)/usr/bin
	curl -fsSL --retry 3 --retry-delay 2 -o $(PKG_BUILD_DIR)/usr/bin/vk-turn-client \
		"https://github.com/kiper292/vk-turn-proxy/releases/download/v$(PKG_VERSION)/client-linux-$(UPSTREAM_ARCH)"
	chmod 0755 $(PKG_BUILD_DIR)/usr/bin/vk-turn-client
endef

define Build/Compile
	:
endef

define Package/vk-turn-client/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/usr/bin/vk-turn-client $(1)/usr/bin/vk-turn-client
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/etc/config/vk-turn-client $(1)/etc/config/vk-turn-client
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/etc/init.d/vk-turn-client $(1)/etc/init.d/vk-turn-client
endef

$(eval $(call BuildPackage,vk-turn-client))