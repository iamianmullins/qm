RPM_TOPDIR ?= $(PWD)/rpmbuild
VERSION ?= $(shell cat VERSION)
ROOTDIR ?= $(PWD)
SPECFILE_SUBPACKAGE_DVB ?= ${ROOTDIR}/rpm/dvb/dvb.spec
PACKAGE_NAME = qm-mount-bind-dvb
NV ?= ${PACKAGE_NAME}-${VERSION}

.PHONY: dist
dist: ##             - Creates the QM dvb package
	cd $(ROOTDIR) && tar cvz \
		--dereference \
		--transform s/qm/${NV}/ \
		-f /tmp/${NV}.tar.gz \
		../qm/README.md \
		../qm/SECURITY.md \
		../qm/LICENSE \
		../qm/etc/containers/systemd/qm.container.d/qm_dropin_mount_bind_dvb.conf
	cd $(ROOTDIR) && mv /tmp/${NV}.tar.gz ./rpm

.PHONY: dvb
dvb: dist ##             - Creates a local RPM package, useful for development
	cd $(ROOTDIR) && mkdir -p ${RPM_TOPDIR}/{RPMS,SRPMS,BUILD,SOURCES}
	cd $(ROOTDIR) && tools/version-update -v ${VERSION}
	cd $(ROOTDIR) && cp ./rpm/${NV}.tar.gz ${RPM_TOPDIR}/SOURCES
	rpmbuild -ba \
		--define="_topdir ${RPM_TOPDIR}" \
		--define="version ${VERSION}" \
		${SPECFILE_SUBPACKAGE_DVB}
	if [ ! -f ${RPM_TOPDIR}/RPMS/noarch/${NV}*.noarch.rpm ]; then \
		echo "RPM build failed"; \
		exit 1; \
	fi

