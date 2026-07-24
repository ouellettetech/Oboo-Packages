#
# Replacement package definition for 'oupgrade'.
#
# Upstream (OnionIoT/oupgrade) moved from a compiled C binary to a plain
# POSIX shell script (oupgrade.sh) on its 'master' branch. The onion feed's
# original Makefile (feeds/onion/oupgrade/Makefile, from
# OnionIoT/OpenWRT-Packages) still expects a compiled binary and fails to
# build against current upstream. This package installs the shell script
# directly instead, matching what upstream now actually provides.
#
# Drop this file at: <Oboo-Packages repo>/oupgrade/Makefile
# Then force-install it ahead of the onion feed's copy:
#   ./scripts/feeds install -f -p oboo oupgrade

include $(TOPDIR)/rules.mk

PKG_NAME:=oupgrade
PKG_VERSION:=0.2
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/OnionIoT/oupgrade.git
# Pin to a known-good commit so this can't silently break again the way the
# unpinned clone in the old onion-feed Makefile did. Update deliberately.
PKG_SOURCE_VERSION:=fdae852db075836cdecdf3ac8c4ce030f150593e
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)

include $(INCLUDE_DIR)/package.mk

define Package/oupgrade
  SECTION:=onion
  CATEGORY:=Onion
  SUBMENU:=Utilities
  TITLE:=Onion firmware upgrade tool
  DEPENDS:=+uclient-fetch +wget +jsonfilter +ubus +uci
endef

define Package/oupgrade/description
 Script to check the current Onion/Oboo firmware version against the
 firmware API and perform an upgrade if a newer version is available.
endef

# No compile step: the upstream project is a shell script, not C source.
define Build/Compile
endef

define Package/oupgrade/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/oupgrade.sh $(1)/usr/bin/oupgrade
endef

$(eval $(call BuildPackage,oupgrade))
