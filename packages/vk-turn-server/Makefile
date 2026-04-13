include $(TOPDIR)/rules.mk

PKG_NAME:=vk-turn-server
PKG_VERSION:=null
PKG_RELEASE:=1

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Auto-builder <auto@github>

include $(INCLUDE_DIR)/package.mk

define Package/vk-turn-server
  SECTION:=net
  CATEGORY:=Network
  TITLE:=VK TURN Proxy Server for OpenWRT
  URL:=https://github.com/kiper292/vk-turn-proxy
  DEPENDS:=+procd-ujail +libc
endef

define Package/vk-turn-server/description
  Server component of VK TURN Proxy for routing WireGuard/Hysteria traffic
  through VK Calls or Yandex Telemost TURN servers.
  
  Binary is automatically downloaded from upstream GitHub Releases.
endef

# Маппинг архитектур OpenWRT → бинарники upstream
define GetUpstreamArch
$(subst mipsel,mipsle,$(subst x86_64,amd64,$(subst aarch64,arm64,$(subst pentium4,386,$(1)))))
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	
	# Определяем архитектуру бинарника
	OPENWRT_ARCH:=$(ARCH)
	UPSTREAM_ARCH:=$(call GetUpstreamArch,$(OPENWRT_ARCH))
	
	# fallback для неизвестных архитектур
	ifeq ($(UPSTREAM_ARCH),)
	  UPSTREAM_ARCH:=$(ARCH)
	endif
	
	# URL для скачивания
	DOWNLOAD_URL:=https://github.com/kiper292/vk-turn-proxy/releases/download/v$(PKG_VERSION)/server-linux-$(UPSTREAM_ARCH)
	
	$(info Downloading server binary: $(DOWNLOAD_URL))
	
	# Скачиваем бинарник напрямую в staging
	mkdir -p $(PKG_BUILD_DIR)/usr/bin
	$(call Download,server-binary,$(DOWNLOAD_URL))
	$(CP) $(DL_DIR)/server-binary $(PKG_BUILD_DIR)/usr/bin/vk-turn-server
	chmod 755 $(PKG_BUILD_DIR)/usr/bin/vk-turn-server
endef

define Build/Compile
	# Бинарник уже готов
	:
endef

define Package/vk-turn-server/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/usr/bin/vk-turn-server $(1)/usr/bin/vk-turn-server
	
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/etc/config/vk-turn-server $(1)/etc/config/vk-turn-server
	
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/etc/init.d/vk-turn-server $(1)/etc/init.d/vk-turn-server
endef

$(eval $(call BuildPackage,vk-turn-server))