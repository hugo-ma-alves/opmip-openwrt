

include $(TOPDIR)/rules.mk

PKG_NAME_SHORT:=opmip
PKG_NAME:=$(PKG_NAME_SHORT)
PKG_VERSION:=1.0

PKG_SOURCE:=master.zip
PKG_SOURCE_URL:= https://github.com/ATNoG/opmip/archive/
PKG_BUILD_DIR:=$(BUILD_DIR)/opmip-master

include $(INCLUDE_DIR)/package.mk

define Package/opmip
  SECTION:=net
  CATEGORY:=Network
  URL:=http://atnog.av.it.pt/odtone
  DEPENDS:=+kmod-ipv6 +boost-thread +boost-system +boost-program_options +libnl
  TITLE:=OPMIP Package
endef

define Package/opmip/description
 This package contains the opmip implementation.
endef

define Package/Prepare
	$(call Build/Prepare/Default)
endef

define Build/Compile
	+( cd $(PKG_BUILD_DIR) ; \
		AR=$(GNU_TARGET_NAME)-ar \
		LD=$(GNU_TARGET_NAME)-ld \
		NM=$(GNU_TARGET_NAME)-nm \
		CC=$(GNU_TARGET_NAME)-gcc \
		GCC=$(GNU_TARGET_NAME)-gcc \
		CXX=$(GNU_TARGET_NAME)-g++ \
		RANLIB=$(GNU_TARGET_NAME)-ranlib \
		STRIP=$(GNU_TARGET_NAME)-strip \
		OBJCOPY=$(GNU_TARGET_NAME)-objcopy \
		OBJDUMP=$(GNU_TARGET_NAME)-objdump \
		b2  app/opmip-lma app/opmip-mag  \
			$(filter -j%,$(PKG_JOBS)) \
			toolset=gcc-$(ARCH) --build-type=minimal --layout=system optimization=space \
			--disable-long-double link=shared runtime-link=shared inlining=on debug-symbols=off \
			$(CONFIGURE_ARGS)  linkflags="-lpthread -lgcc_eh -lboost_program_options -lboost_system -lboost_thread -lnl -lnl-genl -lnl-route" \
			release \
	)
endef

define Package/opmip/install
	
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/bin.v2/lib/opmip/gcc-$(ARCH)/release/inlining-on/optimization-space/libconf.so $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/bin.v2/lib/opmip/gcc-$(ARCH)/release/inlining-on/optimization-space/libopmip.so $(1)/usr/lib/


	$(INSTALL_DIR) $(1)/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin.v2/app/opmip-lma/gcc-$(ARCH)/release/inlining-on/optimization-space/opmip-lma $(1)/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin.v2/app/opmip-mag/gcc-$(ARCH)/release/inlining-on/optimization-space/opmip-mag $(1)/sbin/


	$(INSTALL_DIR) $(1)/etc/opmip
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/node.db $(1)/etc/opmip/

endef

$(eval $(call BuildPackage,opmip))