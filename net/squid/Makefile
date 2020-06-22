#
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=squid
PKG_VERSION:=4.12
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=http://www.squid-cache.org/Versions/v4/
PKG_HASH:=f42a03c8b3dc020722c88bf1a87da8cb0c087b2f66b41d8256c77ee1b527e317

PKG_MAINTAINER:=Marko Ratkaj <marko.ratkaj@sartura.hr>
PKG_LICENSE:=GPL-2.0-or-later
PKG_CPE_ID:=cpe:/a:squid-cache:squid

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/squid/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  URL:=http://www.squid-cache.org/
endef

define Package/squid
  $(call Package/squid/Default)
  MENU:=1
  DEPENDS:=+libpthread +librt +libltdl +libstdcpp +libatomic +USE_GLIBC:libbsd
  DEPENDS+= +SQUID_use-gnutls:libgnutls +SQUID_use-openssl:libopenssl
  DEPENDS+= +SQUID_with-libcap:libcap
  DEPENDS+= +SQUID_with-nettle:libnettle
  DEPENDS+= +SQUID_with-expat:libexpat
  DEPENDS+= +SQUID_with-libxml2:libxml2
  USERID:=squid=137:squid=137
  TITLE:=full-featured Web proxy cache
endef

define Package/squid-full
 $(call Package/squid)
  TITLE += (extended for Jellyfish)
  DEPENDS+= +SQUID_full_use-gnutls:libgnutls +SQUID_full_use-openssl:libopenssl
  DEPENDS+= +SQUID_full_with-libcap:libcap
  DEPENDS+= +SQUID_full_with-nettle:libnettle
  DEPENDS+= +SQUID_full_with-expat:libexpat
  DEPENDS+= +SQUID_full_with-libxml2:libxml2
  VARIANT:=full
  PROVIDES:=squid
endef

define Package/squid/description
  Squid is a caching proxy for the Web supporting HTTP, HTTPS, FTP, and more.
  It reduces bandwidth and improves response times by caching and reusing
  frequently-requested web pages.
endef

define Package/squid-full/description
  $(call Package/squid/description)

 This is a full featured variant with the following compile options enabled:
 --enable-icap-client ...
endef

define Package/squid/config
  source "$(SOURCE)/Config.in"
endef

define Package/squid-full/config
  source "$(SOURCE)/Config-full.in"
endef

define Package/squid/conffiles
  /etc/squid/squid.conf
endef

define Package/squid-mod-cachemgr
  $(call Package/squid/Default)
  DEPENDS:=squid
  TITLE:=Web based proxy manager and reporting tool
endef

CONFIGURE_ARGS += \
	BUILDCXX=$(HOSTCXX) \
	BUILDCXXFLAGS=$(if $(HOST_CXXFLAGS),$(HOST_CXXFLAGS),-O2) \
	--config-cache \
	--datadir=/usr/share/squid \
	--libexecdir=/usr/lib/squid \
	--sysconfdir=/etc/squid \
	--enable-dependency-tracking \
	--enable-shared \
	--enable-epoll \
	--enable-delay-pools \
	--enable-kill-parent-hack \
	--enable-ssl \
	--enable-cache-digests \
	--disable-esi \
	--disable-htcp \
	--disable-static \
	--disable-arch-native \
	--enable-linux-netfilter \
	--without-netfilter-conntrack \
	--disable-pf-transparent \
	--disable-ipf-transparent \
	--disable-ipfw-transparent \
	--disable-ident-lookups \
	--disable-external-acl-helpers \
	--enable-auth \
	--disable-unlinkd \
	--disable-translation \
	--disable-auto-locale \
	--with-dl \
	--with-pthreads \
	--with-filedescriptors=2048 \
	--without-mit-krb5 \
	--without-heimdal-krb5 \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_enable-ipv6),--enable,--disable)-ipv6 \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_enable-http-violations),--enable,--disable)-http-violations \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_enable-x-accelerator-vary),--enable,--disable)-x-accelerator-vary \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_enable-ssl-crtd),--enable-ssl-crtd) \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_enable-icmp),--enable,--disable)-icmp \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_enable-snmp),--enable,--disable)-snmp \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_enable-ecap),--enable,--disable)-ecap \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_enable-icap-client),--enable,--disable)-icap-client \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_auth-basic),--enable,--disable)-auth-basic \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_auth-digest),--enable,--disable)-auth-digest \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_auth-ntlm),--enable,--disable)-auth-ntlm \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_auth-negotiate),--enable,--disable)-auth-negotiate \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_use-gnutls),--with,--without)-gnutls \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_use-gnutls),--without-openssl) \
    $(if $(CONFIG_SQUID_$(BUILD_VARIANT)_use-gnutls),,--with-openssl="$(STAGING_DIR)/usr") \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_with-libcap),--with,--without)-libcap \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_with-nettle),--with,--without)-nettle \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_with-expat),--with,--without)-expat \
	$(if $(CONFIG_SQUID_$(BUILD_VARIANT)_with-libxml2),--with,--without)-libxml2


CONFIGURE_VARS += \
	ac_cv_header_linux_netfilter_ipv4_h=yes \
	ac_cv_epoll_works=yes

TARGET_CFLAGS += -Wno-error
TARGET_LDFLAGS += -latomic

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/lib all
	+$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		install
endef

define Package/squid/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/squid $(1)/usr/sbin/

	$(INSTALL_DIR) $(1)/usr/lib/squid
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/squid/* $(1)/usr/lib/squid/

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/squid.config $(1)/etc/config/squid

	$(INSTALL_DIR) $(1)/etc/squid
	$(CP) $(PKG_INSTALL_DIR)/etc/squid/* $(1)/etc/squid/
	$(INSTALL_CONF) ./files/squid.conf $(1)/etc/squid/

	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/squid.init $(1)/etc/init.d/squid

	$(INSTALL_DIR) $(1)/usr/share/squid/icons/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/squid/icons/* $(1)/usr/share/squid/icons/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/squid/mib.txt $(1)/usr/share/squid/

	$(INSTALL_DIR) $(1)/usr/share/squid/errors/templates/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/squid/errors/templates/* $(1)/usr/share/squid/errors/templates/
endef

define Package/squid-full/install
	$(call Package/squid/install,$(1))
endef

define Package/squid-mod-cachemgr/install
	$(INSTALL_DIR) $(1)/www/cgi-bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/squid/cachemgr.cgi $(1)/www/cgi-bin/
endef

$(eval $(call BuildPackage,squid))
$(eval $(call BuildPackage,squid-full))
$(eval $(call BuildPackage,squid-mod-cachemgr))
