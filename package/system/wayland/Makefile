include $(TOPDIR)/rules.mk

PKG_NAME:=libwayland
PKG_SOURCE_URL:= https://gitlab.freedesktop.org/wayland/wayland
PKG_SOURCE_VERSION:=1.20.93
PKG_SOURCE_PROTO:=git
PKG_RELEASE:=3

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/meson.mk

PKG_BUILD_DEPENDS:=ninja/host meson/host libffi/host libexpat/host libxml2/host

define Package/libwayland
  SECTION:=base
  CATEGORY:=Base system
  DEPENDS:=+libffi
  TITLE:= Wayland library
endef

define Package/libwayland/description
	Display server (compositor) for Nextion basic
endef

define Host/Compile
	cd $(HOST_BUILD_DIR); \
		meson build/ --prefix=$(STAGING_DIR); \
		ninja -C build/ install
endef

define Host/Install
	$(INSTALL_DIR) $(STAGING_DIR)/usr/lib/pkgconfig
	for i in $$$$(ls $(STAGING_DIR)/lib/x86_64-linux-gnu/pkgconfig/*); do \
		$(LN) $$$$i $(STAGING_DIR)/usr/lib/pkgconfig/$$(basename $$i); \
	done
	$(INSTALL_DIR) $(STAGING_DIR)/usr/bin
	$(LN) $(STAGING_DIR)/bin/wayland-scanner $(STAGING_DIR)/usr/bin/wayland-scanner
endef

define Build/Compile
	cd $(PKG_BUILD_DIR);\
		meson build/ --cross-file openwrt-cross.txt --prefix=$(PKG_BUILD_DIR)/root;\
		ninja -C build/ install;

	# Not needed
	$(RM) -r $(PKG_BUILD_DIR)/root/share
	$(RM) -r $(PKG_BUILD_DIR)/root/bin
	$(RM) -r $(PKG_BUILD_DIR)/root/lib/pkgconfig
endef

define Package/libwayland/install
	$(CP) -r $(PKG_BUILD_DIR)/root/* $(1)
endef


$(eval $(call BuildPackage,libwayland))
$(eval $(call HostBuild))

