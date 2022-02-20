

BASEDIR:=$(shell dab basedir)

all: info/init_ok
	dab bootstrap --minimal
	echo "APT::Install-Recommends \"false\";" > ${BASEDIR}/etc/apt/apt.conf.d/00InstallRecommends
	echo "en_US.UTF-8" >${BASEDIR}/etc/locale
	echo "en_US.UTF-8 UTF-8" > ${BASEDIR}/etc/locale.gen
	echo "ru_RU.UTF-8 UTF-8" >> ${BASEDIR}/etc/locale.gen
	dab install locales
	dab exec dpkg-reconfigure -f noninteractive locales
	echo "Europe/Sofia" > ${BASEDIR}/etc/timezone
	dab exec cp /usr/share/zoneinfo/Europe/Sofia /etc/localtime
	dab exec dpkg-reconfigure -f noninteractive tzdata
	
	dab exec dpkg --purge postfix
	dab install bzip2 dialog wget traceroute logrotate net-tools
	sed -i "s/#PermitRootLogin prohibit-password/PermitRootLogin yes/" ${BASEDIR}/etc/ssh/sshd_config
	echo "#!/bin/sh -e" >${BASEDIR}/etc/rc.local
	echo "lsb_release -d -s >/etc/issue" >>${BASEDIR}/etc/rc.local
	echo "printf "\nIP-address: %s\n\n" `hostname -I` >>/etc/issue" >> ${BASEDIR}/etc/rc.local
	echo "exit 0" >> ${BASEDIR}/etc/rc.local
	cp rc-local.service ${BASEDIR}/etc/systemd/system/rc-local.service
	dab exec chmod +x ${BASEDIR}/etc/rc.local
	dab exec systemctl enable rc-local
	dab exec systemctl disable ssh.socket
	dab exec systemctl enable ssh.service
	dab finalize

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
