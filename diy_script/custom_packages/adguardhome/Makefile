#
# 预编译二进制版 AdGuard Home（不再走源码构建）
#
# 改动原因：
#   1) 0.107.25 依赖 quic-go v0.32.0，该版本内置编译期保险 go121.go，
#      在 host golang >= 1.21 下必定报错：
#      cannot use "The version of quic-go you're using can't be built on Go 1.21 yet." as int value
#   2) 源码构建还需要 node + yarn 编译前端（js-deps js-build），拖慢 CI。
#
# 现在直接使用官方 release 的 linux_amd64 包：Web UI 已内嵌进二进制，
# 构建阶段只需解包，不再依赖 host golang / node / yarn。
#

include $(TOPDIR)/rules.mk

PKG_NAME:=adguardhome
PKG_VERSION:=0.107.25
PKG_RELEASE:=1

PKG_SOURCE:=AdGuardHome_linux_amd64.tar.gz
PKG_SOURCE_URL:=https://github.com/AdguardTeam/AdGuardHome/releases/download/v$(PKG_VERSION)
PKG_HASH:=a43d6fb38347581c99984a33760a730570de4b36c4068bb26b0778d4499ff101

PKG_LICENSE:=GPL-3.0-only
PKG_LICENSE_FILES:=LICENSE.txt
PKG_MAINTAINER:=Dobroslaw Kijowski <dobo90@gmail.com>

include $(INCLUDE_DIR)/package.mk

define Package/adguardhome
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Network-wide ads and trackers blocking DNS server
	URL:=https://github.com/AdguardTeam/AdGuardHome
	DEPENDS:=+ca-bundle
endef

define Package/adguardhome/conffiles
/etc/adguardhome.yaml
/etc/config/adguardhome
endef

define Package/adguardhome/description
Free and open source, powerful network-wide ads and trackers blocking DNS server.
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	tar -xzf $(DL_DIR)/$(PKG_SOURCE) -C $(PKG_BUILD_DIR)
	chmod +x $(PKG_BUILD_DIR)/AdGuardHome/AdGuardHome
endef

define Build/Compile
endef

define Package/adguardhome/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/AdGuardHome/AdGuardHome $(1)/usr/bin/AdGuardHome
	
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/adguardhome.init $(1)/etc/init.d/adguardhome
	
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/adguardhome.config $(1)/etc/config/adguardhome
endef

$(eval $(call BuildPackage,adguardhome))