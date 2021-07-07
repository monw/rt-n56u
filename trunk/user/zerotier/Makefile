SRC_NAME = ZeroTierOne-1.6.5
SRC_URL = https://codeload.github.com/zerotier/ZeroTierOne/tar.gz/refs/tags/1.6.5
THISDIR = $(shell pwd)
FLAGS := DEFS="" 
CFLAGS := -O3 -ffunction-sections -fdata-sections
LDFLAGS := -Wl,--gc-sections,--as-needed

all: download_test extract_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)
	
download_test:
	( if [ ! -f $(THISDIR)/$(SRC_NAME).tar.gz ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O $(SRC_NAME).tar.gz $(SRC_URL); \
	fi )

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar xf $(SRC_NAME).tar.gz ; \
		patch -d $(SRC_NAME) -p1 -i ../0002-remove-pie.patch; \
		patch -d $(SRC_NAME) -p1 -i ../010-auxv.patch; \
		patch -d $(SRC_NAME) -p1 -i ../0004-accept-external-linker-flags.patch; \
		patch -d $(SRC_NAME) -p1 -i ../0005-link-natpmp.patch; \
		patch -d $(SRC_NAME) -p1 -i ../0007-add-cerrno-header-for-str-errno.patch; \
	fi )
config_test:

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \

romfs:
	$(ROMFSINST) $(THISDIR)/$(SRC_NAME)/zerotier-one /usr/bin/zerotier-one
	$(ROMFSINST) $(THISDIR)/zerotier.sh /usr/bin/zerotier.sh
	ln -sf zerotier-one $(ROMFSDIR)/usr/bin/zerotier-cli
	ln -sf zerotier-one $(ROMFSDIR)/usr/bin/zerotier-idtool
	cp -fP $(CONFIG_CROSS_COMPILER_ROOT)/mipsel-linux-uclibc/sysroot/lib/libstdc++.so* $(ROMFSDIR)/lib

