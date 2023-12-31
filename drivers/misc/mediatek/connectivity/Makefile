#
# Copyright (C) 2015 MediaTek Inc.
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License version 2 as
# published by the Free Software Foundation.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
# GNU General Public License for more details.
#

# Connectivity combo driver
# If KERNELRELEASE is defined, we've been invoked from the
# kernel build system and can use its language.
ifneq ($(KERNELRELEASE),)
    subdir-ccflags-y += -I$(srctree)/
    subdir-ccflags-y += -I$(srctree)/drivers/misc/mediatek/base/power/include
    subdir-ccflags-y += -I$(srctree)/drivers/misc/mediatek/base/power/include/clkbuf_v1
    subdir-ccflags-y += -I$(srctree)/drivers/misc/mediatek/base/power/include/clkbuf_v1/$(MTK_PLATFORM)
    subdir-ccflags-y += -Werror -I$(srctree)/drivers/misc/mediatek/include/mt-plat/$(MTK_PLATFORM)/include
    subdir-ccflags-y += -Werror -I$(srctree)/drivers/misc/mediatek/include/mt-plat
ifeq ($(CONFIG_MTK_PMIC_CHIP_MT6359),y)
    subdir-ccflags-y += -Werror -I$(srctree)/drivers/misc/mediatek/pmic/include/mt6359
endif
    subdir-ccflags-y += -I$(srctree)/drivers/mmc/core
    subdir-ccflags-y += -I$(srctree)/drivers/misc/mediatek/eccci/$(MTK_PLATFORM)
    subdir-ccflags-y += -I$(srctree)/drivers/misc/mediatek/eccci/
    subdir-ccflags-y += -I$(srctree)/drivers/clk/mediatek/
    subdir-ccflags-y += -I$(srctree)/drivers/pinctrl/mediatek/

    # Do Nothing, move to standalone repo
    MODULE_NAME := connadp
    obj-y += $(MODULE_NAME).o
    $(MODULE_NAME)-objs += common/connectivity_build_in_adapter.o
    $(MODULE_NAME)-objs += common/wmt_build_in_adapter.o


    # Do build-in for Makefile checking
    export CONFIG_WLAN_DRV_BUILD_IN=n

    ifeq ($(CONFIG_WLAN_DRV_BUILD_IN),y)
        PATH_TO_FM_DRV       = vendor/mediatek/kernel_modules/connectivity/fmradio
        PATH_TO_GPS_DRV      = vendor/mediatek/kernel_modules/connectivity/gps

        ABS_PATH_TO_FM_DRV       = $(srctree)/../$(PATH_TO_FM_DRV)
        ABS_PATH_TO_GPS_DRV      = $(srctree)/../$(PATH_TO_GPS_DRV)

        $(warning symbolic link to $(PATH_TO_FM_DRV))
        $(warning symbolic link to $(PATH_TO_GPS_DRV))

        $(shell unlink $(srctree)/$(src)/fm)
        $(shell unlink $(srctree)/$(src)/gps)

        $(shell ln -s $(ABS_PATH_TO_FM_DRV)       $(srctree)/$(src)/fm)
        $(shell ln -s $(ABS_PATH_TO_GPS_DRV)      $(srctree)/$(src)/gps)

        # for gen2 options
        export CONFIG_MTK_COMBO_WIFI_HIF=axi
        export MTK_COMBO_CHIP=CONSYS_6739
        export WLAN_CHIP_ID=6739
        export MTK_ANDROID_WMT=y

        # Do build-in for xxx.c checking
        subdir-ccflags-y += -D MTK_WCN_REMOVE_KERNEL_MODULE
        subdir-ccflags-y += -D MTK_WCN_BUILT_IN_DRIVER
        obj-y += wmt_drv/
        obj-y += wmt_chrdev_wifi/
        obj-y += wlan_drv_gen2/
        obj-y += bt/
        obj-y += fm/
        obj-y += gps/
    endif

# Otherwise we were called directly from the command line;
# invoke the kernel build system.
else
    KERNELDIR ?= /lib/modules/$(shell uname -r)/build
    PWD  := $(shell pwd)
default:
	$(MAKE) -C $(KERNELDIR) M=$(PWD) modules
endif