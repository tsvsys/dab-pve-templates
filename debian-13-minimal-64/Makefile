# in order to build debian 13 version template - you need to have dab 3.7.0 installed on your build host

BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap --minimal
	# disable install recommends for apt
	echo "APT::Install-Recommends \"false\";" > ${BASEDIR}/etc/apt/apt.conf.d/00InstallRecommends
	# configure locale
	echo "en_US.UTF-8" >${BASEDIR}/etc/locale
	echo "en_US.UTF-8 UTF-8" > ${BASEDIR}/etc/locale.gen
	dab install locales
	dab exec dpkg-reconfigure -f noninteractive locales
	# configure timezone
	echo "Etc/UTC" > ${BASEDIR}/etc/timezone
	dab exec cp --remove-destination -- /usr/share/zoneinfo/Etc/UTC /etc/localtime
	dab exec dpkg-reconfigure -f noninteractive tzdata
	# base software
	dab exec dpkg --purge postfix
	dab install dbus bzip2 dialog wget traceroute logrotate net-tools vim-nox python3
	# enable root login over ssh and disable DNS for SSHD
	sed -i "s/#PermitRootLogin prohibit-password/PermitRootLogin yes/" ${BASEDIR}/etc/ssh/sshd_config
	sed -i 's/#UseDNS no/UseDNS no/g' ${BASEDIR}/etc/ssh/sshd_config
	# add rc-local service
	cp rc-local.service ${BASEDIR}/etc/systemd/system/rc-local.service
	install -m 0755 rc.local ${BASEDIR}/etc/rc.local
	dab exec systemctl enable rc-local
	# mask not needed services
	dab exec systemctl mask sys-kernel-config.mount
	dab exec systemctl mask systemd-journald-audit.socket
	dab exec systemctl mask systemd-networkd-wait-online.service
#	dab finalize
	# NOTE: zstd appliances are only supported on PVE >= 7.0, take care on upload!
	dab finalize --compressor zstd-max

info/init_ok: dab.conf
	dab init
	touch $@

.PHONY: clean
clean:
	dab clean
	rm -f *~

.PHONY: dist-clean
dist-clean:
	dab dist-clean
	rm -f *~
