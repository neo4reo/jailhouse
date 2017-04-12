#
# Jailhouse, a Linux-based partitioning hypervisor
#
# Copyright (c) Siemens AG, 2013-2017
#
# Authors:
#  Jan Kiszka <jan.kiszka@siemens.com>
#  Benjamin Block <bebl@mageta.org>
#
# This work is licensed under the terms of the GNU GPL, version 2.  See
# the COPYING file in the top-level directory.
#

define filechk_config_mk
(									\
	echo "\$$(foreach config,\$$(filter CONFIG_%,		\
		\$$(.VARIABLES)), \$$(eval undefine \$$(config)))";	\
	if [ -f $(src)/hypervisor/include/jailhouse/config.h ]; then	\
		sed -e "/^#define \([^[:space:]]*\)[[:space:]]*1/!d"	\
		    -e "s/^#define \([^[:space:]]*\)[[:space:]]*1/\1=y/"\
			$(src)/hypervisor/include/jailhouse/config.h;	\
	fi								\
)
endef

GEN_CONFIG_MK := $(obj)/hypervisor/include/generated/config.mk
export GEN_CONFIG_MK

$(GEN_CONFIG_MK): $(src)/Makefile FORCE
	$(call filechk,config_mk)

define filechk_version
	$(src)/scripts/gen_version_h $(src)/
endef

GEN_VERSION_H := $(obj)/hypervisor/include/generated/version.h
export GEN_VERSION_H

$(GEN_VERSION_H): $(src)/Makefile FORCE
	$(call filechk,version)

# Do not generate files by creating dependencies if we are cleaning up
ifeq ($(filter %/Makefile.clean,$(MAKEFILE_LIST)),)

$(obj)/hypervisor $(obj)/inmates: $(GEN_CONFIG_MK)

$(obj)/driver $(obj)/hypervisor: $(GEN_VERSION_H)

endif

subdir-y := driver hypervisor configs inmates tools

subdir-ccflags-y := -Werror

clean-dirs := Documentation/generated hypervisor/include/generated
