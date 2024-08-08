# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Timothée Ravier <tim@siosm.fr>

_py="python"
_pkg=dulwich
pkgname="${_py}-${_pkg}"
_name="${_pkg}"
pkgver=0.22.1
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
  'git'
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
_htt="https://github.com"
_ns="jelmer"
_url="${_http}/${_ns}/${_pkg}"
source=(
)
b2sums=(
)
[[ "${_git}" == false ]] && \
  source+=(
    "git+${_url}.git#tag=${_pkg}-${pkgver}"
    "${_url}/archive/refs/tags/${_pkg}-${pkgver}.tar.gz"
  ) && \
  b2sums+=(
    '5c263266b7e7205efbe1be1bbbac50258c8229f166961b83fdcdb3d87780c31489886eb83ce2defafe86fedafb027ae334fcc7bfb49d212de96f06e819236800'
  )
[[ "${_git}" == true ]] && \
  source+=(
    "${_url}/archive/refs/tags/${_pkg}-${pkgver}.tar.gz"
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
    "$_name"
  # https://github.com/jelmer/dulwich/issues/1293
  git \
    cherry-pick \
      -n \
      36c2264a621b94a30aea68938fe94059487b9cdf
}

build() {
  cd "$_name"
  "${_py}" \
    -m \
      build \
    --wheel \
    --skip-dependency-check \
    --no-isolation
}

check() {
  cd \
    "${_name}"
  PYTHONPATH="${PWD}/dulwich:${PYTHONPATH}" \
  "${_py}" \
    -m \
      unittest \
    -v \
    tests.test_suite
}

package() {
  cd \
    "${_pkg}"
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

