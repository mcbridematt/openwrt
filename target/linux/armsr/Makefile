# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2017 Yousong Zhou <yszhou4tech@gmail.com>

include $(TOPDIR)/rules.mk

BOARD:=armsr
BOARDNAME:=Arm SystemReady (EFI) compliant
FEATURES:=fpu pci pcie rtc usb boot-part rootfs-part
FEATURES+=cpiogz ext4 ramdisk squashfs targz vmdk

KERNEL_PATCHVER:=6.1

include $(INCLUDE_DIR)/target.mk

DEFAULT_PACKAGES += mkf2fs e2fsprogs
# blkid used for resolving PARTUUID
# in sysupgrade. vfat required for
# mounting ESP partition
DEFAULT_PACKAGES += blkid kmod-fs-vfat

# Overwrite global LINUX_KCONFIG_LIST to add arch/arm64/configs/defconfig before
# armv8 configs, after global generic linux configs and target/board (armsr)
# linux configs
#
# Prefer not creating this condition and put the body in armv8/target.mk, but
# it looks that makefile is called before global target.mk and
# LINUX_KCONFIG_LIST is not set as expected
ifeq ($(SUBTARGET),armv8)
	LINUX_KCONFIG_LIST = \
		$(wildcard \
			$(GENERIC_LINUX_CONFIG) \
			$(LINUX_TARGET_CONFIG) \
			$(LINUX_DIR)/arch/arm64/configs/defconfig \
			$(LINUX_SUBTARGET_CONFIG) \
			$(TOPDIR)/env/kernel-config)
endif

$(eval $(call BuildTarget))
