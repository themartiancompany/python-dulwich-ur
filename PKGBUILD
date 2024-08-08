# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Timothée Ravier <tim@siosm.fr>

_git=false
_py="python"
_pkg=dulwich
pkgname="${_py}-${_pkg}"
pkgver=0.22.1
_commit="0fa636dc03e43477199ad7aefd917b628e93b29d"
pkgrel=1
pkgdesc='Pure-Python implementation of the Git file formats and protocols'
arch=(
  'x86_64'
  'arm'
  'aarch64'
  'armv7l'
  'mips'
  'powerpc'
  'i686'
  'pentium4'
)
url="https://www.${_pkg}.io"
license=(
  'GPL'
)
depends=(
  "${_py}-setuptools"
  "${_py}-urllib3"
)
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools-rust"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-gpgme"
  "${_py}-paramiko"
)
optdepends=(
  "${_py}-fastimport: for fast-import support"
  "${_py}-gpgme: for PGP signature support"
  "${_py}-idna: for HTTPS support via urllib3"
  "${_py}-paramiko: for use as the SSH implementation"
  "${_py}-pyopenssl: for HTTPS support via urllib3"
  "${_py}-pyinotify: to watch for changes to refs"
)
provides=(
  "${_pkg}"
)
_http="https://github.com"
_ns="jelmer"
_url="${_http}/${_ns}/${_pkg}"
source=(
)
b2sums=(
)
[[ "${_git}" == 'false' ]] && \
  source+=(
    "${_url}/archive/refs/tags/${_pkg}-${pkgver}.tar.gz"
  ) && \
  b2sums+=(
    'aa6e5fd283468710173cb096a3537686de9ced834d9a5d984e528e443e55466583c356a29638533bf47aacc514d18df52703a96b5b8b16355a84d6ed7c13fb2b'
  )
[[ "${_git}" == 'true' ]] && \
  makedepends+=(
    git
  )
  source+=(
    "${_pkg}-${pkgver}::git+${_url}.git#commit=${_commit}"
  ) && \
  b2sums+=(
    '5c263266b7e7205efbe1be1bbbac50258c8229f166961b83fdcdb3d87780c31489886eb83ce2defafe86fedafb027ae334fcc7bfb49d212de96f06e819236800'
  )
# validpgpkeys=(
#   Jelmer Vernooĳ <jelmer@jelmer.uk>
#   'DC837EE14A7E37347E87061700806F2BD729A457'
# )

prepare() {
  cd \
    "${_pkg}-${pkgver}"
  # https://github.com/jelmer/dulwich/issues/1293
  git \
    cherry-pick \
      -n \
      36c2264a621b94a30aea68938fe94059487b9cdf
}

build() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --skip-dependency-check \
    --no-isolation
}

check() {
  cd \
    "${_pkg}-${pkgver}"
  PYTHONPATH="${PWD}/dulwich:${PYTHONPATH}" \
  "${_py}" \
    -m \
      unittest \
    -v \
    tests.test_suite
}

package() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl

  # symlink license file
  local \
    site_packages=$( \
      python \
        -c \
	"import site; print(site.getsitepackages()[0])")
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}" || \
    true
  ln \
    -s \
    "${site_packages}/${_pkg}-${pkgver}.dist-info/licenses/COPYING" \
    "${pkgdir}/usr/share/licenses/${pkgname}/COPYING" || \
    true
}

