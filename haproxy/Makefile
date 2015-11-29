include ../Makefile.inc

UPSTREAM=http://www.haproxy.org/download/1.6/src/haproxy-1.6.2.tar.gz
TARBALL=$(notdir $(UPSTREAM))

all: $(RUMPRUN_PKGS_DIR)/lib/libpcre.a bin/haproxy

.NOTPARALLEL: $(RUMPRUN_PKGS_DIR)/lib/libpcre.a
$(RUMPRUN_PKGS_DIR)/lib/libpcre.a:
	$(MAKE) -C ../pcre

bin/haproxy: build/haproxy
	mkdir -p bin
	cp $< $@

build/haproxy: build/Makefile
	$(MAKE) -C build TARGET=netbsd USE_PCRE=1 PCREDIR=$(RUMPRUN_PKGS_DIR) CC=$(RUMPRUN_CC)

build/Makefile: | dl/$(TARBALL)
	mkdir -p build
	(cd build && tar -xz --strip-components 1 -f ../dl/$(TARBALL))

dl/$(TARBALL):
	mkdir -p dl
	../scripts/fetch.sh ${UPSTREAM} dl/$(TARBALL)

.PHONY: clean
clean:
	$(MAKE) -C build clean
	rm -rf bin/*
	rm -rf images/*

.PHONY: distclean
distclean: clean
	rm -rf build
