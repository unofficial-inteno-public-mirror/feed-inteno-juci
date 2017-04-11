
include $(TOPDIR)/rules.mk

PKG_NAME:=juci

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=http://public.inteno.se:/juci
PKG_SOURCE_VERSION:=887b0851ea2a8d8f26a61324ce594abebc98ff18
PKG_VERSION:=2017-04-11
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_SOURCE_VERSION).tar.gz
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)

PKG_RELEASE=$(PKG_VERSION)-$(PKG_SOURCE_VERSION)

PKG_LICENSE:=GPL
PKG_LICENSE_FILES:=

include $(INCLUDE_DIR)/package.mk

define Package/juci
  $(Package/juci/default)
  SECTION:=juci
  CATEGORY:=JUCI
  MENU=1
  TITLE:=JUCI Core Package (select this to select default plugins)
  DEPENDS:=+libubox +libubus +owsd +rpcd +rpcd-mod-file +questd
endef

define Build/InstallDev 
	$(INSTALL_DIR) $(1)/usr/src/
	ln -s $(PKG_BUILD_DIR) $(1)/usr/src/juci
endef 

define Build/Compile
	$(call Build/Compile/Default,THEME_PATH="$(TOPDIR)/themes/")
endef

define Package/juci/description
 Provides the JUCI Javascript UCI web interface with standard functionality.
endef

define Package/juci/install
	$(INSTALL_DIR) $(1)/
	$(CP) $(PKG_BUILD_DIR)/bin/juci/* $(1)/
	$(INSTALL_DIR) $(1)/www/cgi-bin/
	$(INSTALL_DIR) $(1)/sbin/
	$(INSTALL_DIR) $(1)/etc/
	$(CP) ./files/etc/* $(1)/etc/
	$(INSTALL_DIR) $(1)/www/
	$(CP) ./files/www/* $(1)/www/
endef

define Package/juci/postinst
#!/bin/sh

if [ "$$(uci -q get uhttpd.main.ubus_prefix)" != "/ubus" ]; then
	uci set uhttpd.main.ubus_prefix="/ubus"
	uci commit uhttpd
fi

juci-update
exit 0
endef

$(eval $(call BuildPackage,juci))

####### Extensions / Themes and Plugins

define RegisterAddonInner
define Package/$(2)
	SECTION:=juci
	CATEGORY:=JUCI
	TITLE:=default
	SUBMENU:=$(1)
	TITLE:=$(if $(3),$(3),JUCI $(2) plugin)
	DEPENDS:=$(4)
endef

define Package/$(2)/install
	$(INSTALL_DIR) $$(1)/
	$(CP) $(PKG_BUILD_DIR)/bin/$(2)/* $$(1)/
endef
	
define Package/$(2)/config
	$(5)
	config $(shell echo $(2) | sed 's/-/_/g')
		bool 
		default n
endef
endef

define RegisterAddon
	$(eval $(call RegisterAddonInner,$(1),$(2),$(3),$(4),$(5)))
	$(eval $(call BuildPackage,$(2)))
endef

$(eval $(call RegisterAddon,Plugins,juci-broadcom-dsl, Inteno Broadcom DSL Module (xdslctl based),+juci-ethernet))
$(eval $(call RegisterAddon,Plugins,juci-broadcom-ethernet, Inteno Broadcom Ethernet Module,+juci-ethernet))
$(eval $(call RegisterAddon,Plugins,juci-broadcom-iptv, Inteno Broadcom IPTV/MCPD Plugin,+juci-network-netifd +juci-ethernet))
$(eval $(call RegisterAddon,Plugins,juci-broadcom-pwrmgmt, Inteno Broadcom Power management Plugin))
$(eval $(call RegisterAddon,Plugins,juci-broadcom-vlan, Inteno Broadcom VLAN Module (vlanctl based),+juci-ethernet))
$(eval $(call RegisterAddon,Plugins,juci-broadcom-xdsl, Inteno Broadcom xDSL Module ))
$(eval $(call RegisterAddon,Plugins,juci-catv, Inteno CATV Module))
$(eval $(call RegisterAddon,Plugins,juci-cgroups,Cgroups module))
$(eval $(call RegisterAddon,Plugins,juci-ddns, DDNS configuration through /etc/config/ddns))
$(eval $(call RegisterAddon,Plugins,juci-diagnostics, Simple Diagnostics ))
$(eval $(call RegisterAddon,Plugins,juci-dnsmasq-dhcp, DHCP/DNSMasq configuration through /etc/config/dhcp ))
$(eval $(call RegisterAddon,Plugins,juci-dropbear, Dropbear Configuration,dropbear))
$(eval $(call RegisterAddon,Plugins,juci-ethernet, JUCI ethernet core module ))
$(eval $(call RegisterAddon,Plugins,juci-event, JUCI Event Subsystem ))
$(eval $(call RegisterAddon,Plugins,juci-firewall-fw3, FW3 Firewall Configuration /etc/config/firewall ))
$(eval $(call RegisterAddon,Plugins,juci-icwmp, Configure TR69 settings /etc/config/icwmp ))
$(eval $(call RegisterAddon,Plugins,juci-igmpinfo, IGMP Snooping Info through kernel snooping tables ))
$(eval $(call RegisterAddon,Plugins,juci-inteno-backup,Inteno Backup Module))
$(eval $(call RegisterAddon,Plugins,juci-inteno-multiwan, Package for configuring inteno MultiWAN))
$(eval $(call RegisterAddon,Plugins,juci-inteno-provisioning,Inteno Provisioning Module))
$(eval $(call RegisterAddon,Plugins,juci-inteno-qos,Inteno QoS module))
$(eval $(call RegisterAddon,Plugins,juci-inteno-voice-client, Inteno Asterisk Voice Client Module))
$(eval $(call RegisterAddon,Plugins,juci-minidlna, MiniDLNA Configuration))
$(eval $(call RegisterAddon,Plugins,juci-mod-status, Deprecated JUCI Status Reporting Module))
$(eval $(call RegisterAddon,Plugins,juci-mod-system, JUCI System Administration Module))
$(eval $(call RegisterAddon,Plugins,juci-mwan3, Package for configuring mwan3 package))
$(eval $(call RegisterAddon,Plugins,juci-natalie-dect, Natalie DECT interface))
$(eval $(call RegisterAddon,Plugins,juci-netmode, Inteno NetMode default reset functionality))
$(eval $(call RegisterAddon,Plugins,juci-network-netifd, OpenWRT netifd network configuration module))
$(eval $(call RegisterAddon,Plugins,juci-owsd, OWSD configuration module))
$(eval $(call RegisterAddon,Plugins,juci-printer, Inteno printer configuration page))
$(eval $(call RegisterAddon,Plugins,juci-realtime-graphs,Realtime Graphs based on luci-bwc,+luci-bwc))
$(eval $(call RegisterAddon,Plugins,juci-samba, Samba36 Configuration))
$(eval $(call RegisterAddon,Plugins,juci-sfp, SFP Configuration))
$(eval $(call RegisterAddon,Plugins,juci-snmpd,JUCI SNMP plugin))
$(eval $(call RegisterAddon,Plugins,juci-sysupgrade, Inteno Sysupgrade Firmware Upgrade))
$(eval $(call RegisterAddon,Plugins,juci-uhttpd,UHTTPD configuration))
$(eval $(call RegisterAddon,Plugins,juci-upnp, UPNP Configuration Module))
$(eval $(call RegisterAddon,Plugins,juci-usb, USB Support In GUI))
$(eval $(call RegisterAddon,Plugins,juci-wireless, Inteno Broadcom Wireless Module (wlctl),+juci-ethernet ))
$(eval $(call RegisterAddon,Themes,juci-theme-inteno))

####### dynamically publish themes as packages #######
$(foreach th,$(wildcard $(TOPDIR)/themes/*),$(eval $(call RegisterAddon,Themes,$(notdir $(th)))))
