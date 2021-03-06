# Maintainer: hamkido <hamki.do2000@gmail.com>
pkgname=mozilla-firefox-sync-server
pkgver=1.9.1
pkgrel=1
epoch=
pkgdesc="This is an all-in-one package for running a self-hosted Firefox Sync server"
arch=('any')
url="https://github.com/mozilla-services/syncserver"
license=('MPL2')
groups=()
depends=('python2-virtualenv')
makedepends=('mysql')
checkdepends=()
optdepends=()
provides=()
conflicts=()
replaces=()
backup=()
options=()
install="${pkgname}.install"
changelog=
source=($pkgname-$pkgver.tar.gz::"https://github.com/mozilla-services/syncserver/archive/$pkgver.tar.gz"
        'ffsync.service'
        'ffsync.tmpfiles')
noextract=()
md5sums=()
sha256sums=('dd12e7b4d97052ab5227151886a92ecbc368a987c4ad5fef7a9b759197a86c1f'
  '8664ad8361d6751aad47e86900270d2efd8b65d520248cd1c164432baba42212'
  '462bfdccc672339a03622dbe0a76a2df1b4293de8b240e82fe127a6befaa1a89')
backup=('opt/mozilla-firefox-sync-server/syncserver.ini')
optdepends=('uwsgi-plugin-python2: Serve the webapp using uwsgi')

prepare() {
  cd "syncserver-${pkgver}"
  sed -i "s|/tmp/syncserver.db|/var/lib/ffsync/sync_storage.db|g" syncserver.ini
  sed -i "0,/^\#sqluri/s//sqluri/" syncserver.ini
  sed -i "s| --no-site-packages||g" Makefile
}

build() {
  cd "syncserver-${pkgver}"
  make build
  rm -rf .git .gitignore Dockerfile Makefile MANIFEST.in README.rst setup.py \
    local/bin/pep8 local/bin/build* local/bin/easy_install* local/bin/pip* \
    local/COMPLETE
  find . -name '*.pyc' -delete
  find . -type f -exec sed -i "s|${srcdir}/syncserver-${pkgver}|/opt/${pkgname}|g" {} \;
}

check() {
  cd "syncserver-${pkgver}"
 # make test
}

package() {
  cd "syncserver-${pkgver}"
  install -dm 755 "${pkgdir}"/opt/${pkgname} "${pkgdir}"/var/lib/ffsync
  cp -a * "${pkgdir}"/opt/${pkgname}
  cd "${pkgdir}"/opt/${pkgname}
  find . -exec chmod go-w {} \;
  find . -type f -exec chmod a+r {} \;
  install -Dm 644 "${srcdir}"/ffsync.service "${pkgdir}"/usr/lib/systemd/system/ffsync.service
  install -Dm 644 "${srcdir}"/ffsync.tmpfiles "${pkgdir}"/usr/lib/tmpfiles.d/ffsync.conf
}
