# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Timothée Ravier <tim@siosm.fr>

pkgname=python-dulwich
_name=${pkgname#python-}
pkgver=0.21.7
pkgrel=2
pkgdesc='Pure-Python implementation of the Git file formats and protocols'
arch=('x86_64')
url=https://www.dulwich.io
license=('GPL')
depends=('python-urllib3')
makedepends=(
  'git'
  'python-build'
  'python-installer'
  'python-setuptools'
  'python-wheel'
)
checkdepends=('python-gpgme' 'python-paramiko')
optdepends=(
  'python-fastimport: for fast-import support'
  'python-gpgme: for PGP signature support'
  'python-idna: for HTTPS support via urllib3'
  'python-paramiko: for use as the SSH implementation'
  'python-pyopenssl: for HTTPS support via urllib3'
  'python-pyinotify: to watch for changes to refs'
)
source=("git+https://github.com/jelmer/dulwich.git#tag=dulwich-$pkgver?signed")
b2sums=('d0883536089a129f96fe3e2b00e90d9eb8e1ec70dc2637acccbe4102a045ef77e3b2ce0cce2ebb6ccef3ec54d37687707745fb11e73e099291ab49cae64d4c59')
validpgpkeys=('DC837EE14A7E37347E87061700806F2BD729A457') # Jelmer Vernooĳ <jelmer@jelmer.uk>

build() {
  cd "$_name"
  python -m build --wheel --skip-dependency-check --no-isolation
}

check() {
  cd "$_name"
  PYTHONPATH="$PWD/dulwich:$PYTHONPATH" python -m unittest dulwich.tests.test_suite
}

package() {
  cd "$_name"
  python -m installer --destdir="$pkgdir" dist/*.whl
}
