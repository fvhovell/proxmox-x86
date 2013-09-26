RELEASE=3.0

PKGVERSION=0.0.4
PKGRELEASE=2
PACKAGE=omping
PKGDIR=${PACKAGE}-${PKGVERSION}
PKGSRC=${PACKAGE}_${PKGVERSION}.orig.tar.gz

ARCH:=$(shell dpkg-architecture -qDEB_BUILD_ARCH)
GITVERSION:=$(shell cat .git/refs/heads/master)

DEBS= ${PACKAGE}_${PKGVERSION}-${PKGRELEASE}_${ARCH}.deb

all: ${DEBS}

${DEBS}: ${PKGSRC}
	rm -rf ${PKGDIR}
	tar xf ${PKGSRC} 
	cp -a debian ${PKGDIR}/debian
	echo "git clone git://git.proxmox.com/git/pve-omping.git\\ngit checkout ${GITVERSION}" > ${PKGDIR}/debian/SOURCE
	cd ${PKGDIR}; dpkg-buildpackage -rfakeroot -b -us -uc

.PHONY: upload
upload: ${DEBS}
	umount /pve/${RELEASE}; mount /pve/${RELEASE} -o rw
	mkdir -p /pve/${RELEASE}/extra
	rm -f /pve/${RELEASE}/extra/${PACKAGE}_*.deb
	rm -f /pve/${RELEASE}/extra/Packages*
	cp ${DEBS} /pve/${RELEASE}/extra
	cd /pve/${RELEASE}/extra; dpkg-scanpackages . /dev/null > Packages; gzip -9c Packages > Packages.gz
	umount /pve/${RELEASE}; mount /pve/${RELEASE} -o ro

distclean: clean

.PHONY: clean
clean:
	rm -rf *~ *_${ARCH}.deb *.changes *.dsc ${PKGDIR} 

.PHONY: dinstall
dinstall: ${DEBS}
	dpkg -i ${DEBS}
