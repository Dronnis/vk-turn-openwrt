include $(TOPDIR)/rules.mk

PKG_NAME:=vk-turn-client
PKG_VERSION:=2.0.6
PKG_RELEASE:=1

PKG_LICENSE:=MIT
PKG_MAINTAINER:=Auto-builder <auto@github>

include $(INCLUDE_DIR)/package.mk

define Package/vk-turn-client
  SECTION:=net
  CATEGORY:=Network
  TITLE:=VK TURN Proxy Client for OpenWRT
  URL:=https://github.com/kiper292/vk-turn-proxy
  DEPENDS:=+procd-ujail +libc
endef

define Package/vk-turn-client/description
  Client component of VK TURN Proxy.
  Binary is automatically downloaded from upstream GitHub Releases.
endef

define GetUpstreamArch
$(subst x86_64,amd64,$(subst aarch64,arm64,$(subst mipsel,mipsle,$(subst i386,386,$(1)))))
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)/usr/bin
	
	OPENWRT_ARCH:=$(ARCH)
	UPSTREAM_ARCH:=$(call GetUpstreamArch,$(OPENWRT_ARCH))
	ifeq ($(UPSTREAM_ARCH),)
	  UPSTREAM_ARCH:=$(ARCH)
	endif
	
	DOWNLOAD_URL:=https://github.com/kiper292/vk-turn-proxy/releases/download/v$(PKG_VERSION)/client-linux-$(UPSTREAM_ARCH)
	$(info 🔽 Downloading client binary: $(DOWNLOAD_URL))
	
	curl -fSL --retry 3 --retry-delay 5 -o $(PKG_BUILD_DIR)/usr/bin/vk-turn-client "$(DOWNLOAD_URL)"
	chmod 755 $(PKG_BUILD_DIR)/usr/bin/vk-turn-client
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