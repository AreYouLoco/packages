#
# Copyright (C) 2019 Jakub Piotr Cłapa <jpc@loee.pl>
# Copyright (C) 2020 Alexandru Ardelean <ardeleanalex@gmail.com>
#

include $(TOPDIR)/rules.mk

PKG_NAME:=numpy
PKG_VERSION:=1.21.2
PKG_RELEASE:=$(AUTORELEASE)

PYPI_NAME:=$(PKG_NAME)
PKG_HASH:=423216d8afc5923b15df86037c6053bf030d15cc9e3224206ef868c2d63dd6dc

PKG_MAINTAINER:=Alexandru Ardelean <ardeleanalex@gmail.com>

PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE.txt
PKG_CPE_ID:=cpe:/a:numpy:numpy

PKG_CONFIG_DEPENDS:= \
        CONFIG_NUMPY_OPENBLAS_SUPPORT

# yes, zip... sigh
PYPI_SOURCE_EXT:=zip
HOST_PYTHON3_PACKAGE_BUILD_DEPENDS:=Cython

include ../pypi.mk
include $(INCLUDE_DIR)/package.mk
include ../python3-package.mk

define Package/python3-numpy
  SUBMENU:=Python
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=The fundamental package for scientific computing with Python
  URL:=https://www.numpy.org/
  DEPENDS:=@!SOFT_FLOAT +INSTALL_GFORTRAN:libgfortran +python3 \
	   +NUMPY_OPENBLAS_SUPPORT:openblas
endef

# We need to duplicate the omitted arhitectures same as openblas
define Package/python3-numpy/config
menu "Configuration"
depends on !powerpc
depends on !arc

config NUMPY_OPENBLAS_SUPPORT
        bool "Enable OpenBLAS support"
        default n

endmenu
endef

ifdef CONFIG_NUMPY_OPENBLAS_SUPPORT
define Build/Prepare/numpy-sitecfg
	echo "[DEFAULT]"                                 >  $(PKG_BUILD_DIR)/site.cfg
	echo "library_dirs = $(STAGING_DIR)/usr/lib"     >> $(PKG_BUILD_DIR)/site.cfg
	echo "include_dirs = $(STAGING_DIR)/usr/include" >> $(PKG_BUILD_DIR)/site.cfg
	echo                                             >> $(PKG_BUILD_DIR)/site.cfg
	echo "[openblas]"                                >> $(PKG_BUILD_DIR)/site.cfg
	echo "libraries = openblas"                      >> $(PKG_BUILD_DIR)/site.cfg
	echo "runtime_library_dirs = /usr/lib"           >> $(PKG_BUILD_DIR)/site.cfg
endef
endif

define Build/Prepare
	$(call Build/Prepare/Default)
	$(call Build/Prepare/numpy-sitecfg)
endef

define Package/python3-numpy/description
NumPy is the fundamental package for array computing with Python.

By default, this package is built without some modules.
For some modules to be available, the INSTALL_GFORTRAN symbol needs
to be enabled in the OpenWrt core/toolchain.
endef

$(eval $(call Py3Package,python3-numpy))
$(eval $(call BuildPackage,python3-numpy))
$(eval $(call BuildPackage,python3-numpy-src))
