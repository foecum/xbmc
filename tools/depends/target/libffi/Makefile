include ../../Makefile.include
DEPS= ../../Makefile.include Makefile

# lib name, version
LIBNAME=libffi
VERSION=3.2.1
SOURCE=$(LIBNAME)-$(VERSION)
ARCHIVE=$(SOURCE).tar.gz

# configuration settings
CONFIGURE= ./configure --prefix=$(PREFIX) --disable-shared --disable-builddir 
ifeq ($(OS), ios)
ifneq ($(CPU), arm64)
CONFIGURE+=CCASFLAGS=-no-integrated-as
endif
endif


LIBDYLIB=$(PLATFORM)/.libs/$(LIBNAME).a

all: .installed-$(PLATFORM)

$(TARBALLS_LOCATION)/$(ARCHIVE):
	cd $(TARBALLS_LOCATION); $(RETRIEVE_TOOL) $(RETRIEVE_TOOL_FLAGS) $(BASE_URL)/$(ARCHIVE)

$(PLATFORM): $(TARBALLS_LOCATION)/$(ARCHIVE) $(DEPS)
	rm -rf $(PLATFORM); mkdir -p $(PLATFORM)
	cd $(PLATFORM); $(ARCHIVE_TOOL) $(ARCHIVE_TOOL_FLAGS) $(TARBALLS_LOCATION)/$(ARCHIVE)
	cd $(PLATFORM); $(CONFIGURE)

$(LIBDYLIB): $(PLATFORM)
	$(MAKE) -j 1 -C $(PLATFORM)
	touch $@

.installed-$(PLATFORM): $(LIBDYLIB)
	$(MAKE) -C $(PLATFORM) install
	mkdir -p $(PREFIX)/include/ffi
	cp $(PREFIX)/lib/$(SOURCE)/include/* $(PREFIX)/include/ffi/
	touch $@

clean:
	$(MAKE) -C $(PLATFORM) clean
	rm -f .installed-$(PLATFORM)

distclean::
	rm -rf $(PLATFORM) .installed-$(PLATFORM)
