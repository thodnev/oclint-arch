# Maintainer: Tymofii Khodniev <thodnev@xinity.devd>

pkgbase='oclint'
pkgname=("${pkgbase}" "${pkgbase}-json-compilation-database")
pkgver=20.11
pkgrel=0
pkgdesc="A static source code analysis tool to improve quality and reduce
         defects for C, C++ and Objective-C"
arch=('x86_64')
url="http://oclint.org/"
license=('BSD')
provides=("${pkgbase}")
conflicts=("${pkgbase}-git")
depends=('clang' 'llvm-libs' 'gcc-libs' 'glibc' 'xz' 'libffi' 'libedit' 'libxml2' 'icu' 'zlib')
makedepends=('gcc' 'binutils' 'make' 'cmake' 'llvm' 'ninja' 'git' 'python')
options=(!ccache)
source=(
    "${pkgbase}::git+https://github.com/oclint/oclint#tag=v${pkgver}"
    "${pkgbase}-json-compilation-database::git+https://github.com/oclint/oclint-json-compilation-database.git"
    "arch-llvm.patch"
)
sha256sums=(
    "SKIP"
    "SKIP"
    "764f44e5af361b851938a733bd6d06d5b1e8badbd622f5c58cd249a1d02613ca"
)

prepare() {
    cd "${srcdir}/${pkgname}"
    patch --strip=1 -i "${srcdir}/arch-llvm.patch"
}

build() {
  cd "${srcdir}/${pkgbase}"
  jflag=`python -c "from multiprocessing import cpu_count; \
         from os import environ; \
         m=environ.get('MAKEFLAGS', f'-j{cpu_count() + 1}'); \
         m=[v for v in m.split(' ') if v.startswith('-j')]; \
         print(m[-1])"`
  # run build script (with system llvm)
  # providing parallelism as set in MAKEFLAGS in makepkg.conf
  ./oclint-scripts/build ${jflag} --llvm-root=$(llvm-config --prefix) --clean --release
}

package_oclint() {
  cd "${srcdir}/${pkgbase}"

  # Run bundle scripts
  ./oclint-scripts/bundle --llvm-root=$(llvm-config --prefix) --release
  
  cd ./build/oclint-release
  
  install -Dm644 ./LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -Dm755 ./lib/oclint/rules/* -t "${pkgdir}/usr/lib/oclint/rules"
  install -Dm755 ./lib/oclint/reporters/* -t "${pkgdir}/usr/lib/oclint/reporters"
  install -Dm755 ./bin/oclint-* "${pkgdir}/usr/bin/oclint"
}

package_oclint-json-compilation-database() {
  depends=("${pkgbase}" 'python')
  pkgdesc="Helps extract and filter information from compile_commands.json,
           and invoke OCLint under the hook for analysis"
  provides=("${pkgbase}-json-compilation-database")
  conflicts=("${pkgbase}-json-compilation-database-git")
  cd "${srcdir}/${pkgbase}-json-compilation-database"
  install -Dm644 ./LICENSE "${pkgdir}/usr/share/licenses/${pkgbase}-json-compilation-database/LICENSE"
  install -Dm755 ./oclint-json-compilation-database "${pkgdir}/usr/bin/oclint-json-compilation-database"
}
