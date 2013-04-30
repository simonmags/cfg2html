product = cfg2html
psf_file = packaging/$(product).psf
depot_dir = /tmp/$(product).depot.dir

all: help

help:
	@echo "+-------------------------+"
	@echo "|    cfg2html Makefile    |"
	@echo "|    =================    |"
	@echo "|    build: \"make depot\"  |"
	@echo "+-------------------------+"

clean:
	@if [ `whoami` != "root" ]; then echo "Only root may remove packages"; exit 1; fi; \
	rm -rf $(depot_dir) ; \
	echo "Depot directory [$(depot_dir)] successfully removed"

lib-tmp:
	@if [ -d ./lib ]; then rm -rf lib-tmp; mkdir -m 755 lib-tmp; \
	cp lib/* lib-tmp/ ; fi

depot: clean lib-tmp
	@/usr/sbin/swpackage -vv -s $(psf_file) -x layout_version=1.0 -d $(depot_dir) ; \
	rel=`/usr/bin/grep 'number' $(psf_file) | awk '{print $$2}'` ; \
	ymd=`date +%Y%m%d` ; \
	depot_file="/tmp/$(product)_"$$rel"_"$$ymd".depot" ; \
	/opt/ignite/bin/make_bundles -b -i -n "CFG2HTML" -t "Config To HTML System Documentation Tool (cfg2html) for HP-UX" \
	-o /tmp/cfg2html_bundle.psf -r $$rel $(depot_dir) ; \
	/usr/sbin/swpackage -s /tmp/cfg2html_bundle.psf -xlayout_version=1.0 -xreinstall_files=true -d  $(depot_dir) ; \
	/usr/sbin/swpackage -vv -d $$depot_file  -x target_type=tape -s $(depot_dir) \* ; \
	echo "File depot location is $$depot_file" ; rm -rf lib-tmp \
	rm -f /tmp/cfg2html_bundle.psf ; \
	echo "Done."
