# Maintainer: f0ruD <fzerorubigd AT Gmail Dot Com>


pkgname=obfuscated-openssh
pkgver=5.2p1
pkgrel=2
pkgdesc='Free version of the SSH connectivity tools, with obfuscated path'
arch=('i686' 'x86_64')
license=('custom:BSD')
url='http://nihilex.com/obfuscated-openssh'
#backup=('opt/etc/ssh/ssh_config' 'opt/etc/ssh/sshd_config' 'opt/etc/pam.d/sshd' 'opt/etc/conf.d/sshd')
depends=('krb5' 'openssl' 'libedit')
optdepends=('x11-ssh-askpass: input passphrase in X without a terminal')
source=("http://socks.nihilex.com/brl-obfuscated-openssh-7288432890c63dc228afe6c61e6343b72ef30962.tar.gz"
        'sshd')
md5sums=('a19490870c17b528d0f7e60e043adb99'
         'ab4eedeee538c0910d85f10f86f17e78')

build() {
	cd "${srcdir}/brl-obfuscated-openssh-7288432890c63dc228afe6c61e6343b72ef30962"

	./configure \
		--prefix=/opt/ob-openssh \
		--with-privsep-user=nobody \
		--with-md5-passwords \
		--with-pam \
		--with-mantype=man \
		--with-xauth=/usr/bin/xauth \
		--with-kerberos5=/usr \
		--with-ssl-engine \
		--with-libedit=/usr/lib \
		--disable-strip # stripping is done by makepkg
	make
}

package() {
	cd "${srcdir}/brl-obfuscated-openssh-7288432890c63dc228afe6c61e6343b72ef30962"
	make DESTDIR="${pkgdir}" install

	install -Dm755 ../sshd "${pkgdir}"/etc/rc.d/obsshd
#	install -Dm644 ../sshd.pam "${pkgdir}"/etc/pam.d/sshd
#	install -Dm644 ../sshd.confd "${pkgdir}"/etc/conf.d/sshd
#	install -Dm644 LICENCE "${pkgdir}/usr/share/licenses/${pkgname}/LICENCE"

#	rm "${pkgdir}"/usr/share/man/man1/slogin.1
#	ln -sf ssh.1.gz "${pkgdir}"/usr/share/man/man1/slogin.1.gz

	# additional contrib scripts that we like
#	install -Dm755 contrib/findssl.sh "${pkgdir}"/usr/bin/findssl.sh
#	install -Dm755 contrib/ssh-copy-id "${pkgdir}"/usr/bin/ssh-copy-id
#	install -Dm644 contrib/ssh-copy-id.1 "${pkgdir}"/usr/share/man/man1/ssh-copy-id.1

	# PAM is a common, standard feature to have
	sed \
		-e '/^#ChallengeResponseAuthentication yes$/c ChallengeResponseAuthentication no' \
		-e '/^#UsePAM no$/c UsePAM yes' \
		-i "${pkgdir}"/opt/ob-openssh/etc/sshd_config
		
	echo "ObfuscatedPort 587" >> "${pkgdir}"/opt/ob-openssh/etc/sshd_config
	echo "ObfuscateKeyword bita" >> "${pkgdir}"/opt/ob-openssh/etc/sshd_config
}
