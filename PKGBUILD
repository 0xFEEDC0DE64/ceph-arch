# Maintainer: Thore Bödecker <foxxx0@archlinux.org>
# Contributor: Sébastien "Seblu" Luttringer <seblu@archlinux.org>

pkgbase='ceph'
pkgname=('ceph' 'ceph-libs' 'ceph-mgr')
_zstdver=1.5.2
pkgver=17.2.0
pkgrel=1
pkgdesc='Distributed, fault-tolerant storage platform delivering object, block, and file system'
arch=('x86_64')
url='https://ceph.com/'
license=('LGPL')
makedepends=('arrow' 'bash' 'bc' 'boost' 'boost-libs' 'bzip2' 'c-ares' 'cmake'
             'coreutils' 'coffeescript' 'cpio' 'cryptsetup' 'cunit' 'curl'
             'cython' 'expat' 'fcgi' 'fontconfig' 'fuse3' 'fmt' 'gcc-libs'
             'glibc' 'gmock' 'gperf' 'gperftools' 'gtest' 'inetutils'
             'java-runtime' 'jq' 'jre-openjdk-headless' 'junit' 'keyutils'
             'libaio' 'libcap-ng' 'libnl' 'librdkafka' 'liburing' 'libxml2'
             'lua' 'lz4' 'nasm' 'ncurses' 'oath-toolkit' 'openssl' 'parted'
             'protobuf' 'procps-ng' 'python-attrs' 'python-bcrypt'
             'python-cheroot' 'python-cherrypy' 'python-dateutil'
             'python-elasticsearch' 'python-flask' 'python-protobuf'
             'python-xmlsec' 'python-yaml' 'python-more-itertools'
             'python-numpy' 'python-pecan' 'python-pip' 'python-pluggy'
             'python-portend' 'python-prettytable' 'python-prometheus_client'
             'python-py' 'python-pyjwt' 'python-pyopenssl' 'python-pytz'
             'python-requests' 'python-routes' 'python-scikit-learn'
             'python-scipy' 'python-setuptools' 'python-sphinx'
             'python-virtualenv' 'python-werkzeug' 'python-wrapt' 'rocksdb' 'snappy' 
             'sqlite' 'systemd' 'util-linux' 'xfsprogs' 'xmlstarlet' 'xmlsec'
             "zstd=${_zstdver}")
checkdepends=('python-coverage' 'python-mock' 'python-nose'
              'python-pycodestyle' 'python-pylint' 'python-pytest'
              'python-pytest-cov' 'socat')
options=('emptydirs')
source=(
  "https://download.ceph.com/tarballs/${pkgbase}-${pkgver}.tar.gz"
  'ceph.sysusers'
  #'glibc2.32-strsignal-compat-backported.patch'
  'ceph-14.2.0-cflags.patch'
  'ceph-12.2.4-boost-build-none-options.patch'
  # 'ceph-13.2.0-cflags.patch' # only needed when building boost
  'ceph-13.2.2-dont-install-sysvinit-script.patch'
  # 'ceph-14.2.0-link-crc32-statically.patch'
  # 'ceph-14.2.0-cython-0.29.patch' # https://github.com/ceph/ceph/commit/38e7686a37373e993210b0c6554d52e281d95c19 https://github.com/ceph/ceph/commit/3bde34af8a84d76744887b2cf10ee4ef36e3925b
  # 'ceph-15.2.0-rocksdb-cmake.patch' # not needed when using system Rocks instead of building it
  # 'ceph-15.2.4-system-uring.patch' # replaced by patch below
  ceph-17.2.0-system-uring.patch::https://src.fedoraproject.org/rpms/ceph/raw/rawhide/f/0008-cmake-modules-Finduring.cmake.patch
  # 'ceph-15.2.5-missing-includes.patch' # applied upstream https://github.com/ceph/ceph/commit/f16ac13c13eceed7adb5a4a04a3372b921e63031
  'disable-empty-readable.sh-test.patch'
  # 'qa-src-update-mypy-to-0.782.patch' # applied upstream https://github.com/ceph/ceph/commit/78f3473f55afe14698dc702e24cf34030223a373
  # 'fix-mgr-dashboard-partial_dict.patch' update patch ?

  # snappy 1.1.9 removed major parts from their namespace, including the
  # snappy::uint32 which was an alias for std::uint32_t
  # 'fix_snappy_namespace_uint.patch' # applied upstream https://github.com/ceph/ceph/commit/4c13a798dcf2e783afd7558bf3541dc025de854a

  # Add python >= 3.8 workaround logic for incompatible modules
  # This has been designated for upstream backporting into the octupus (15) and pacific (16) branches.
  # TODO: check if merged and included in next releases
  # https://tracker.ceph.com/issues/45147
  # https://github.com/ceph/ceph/pull/34846
  # 'backport_mgr_disabled_modules_workaround_PR34846.patch' # applied upstream https://github.com/ceph/ceph/commit/5ddda38da4f3c8ab8ff9c1a501bcd54df558fb80

  # Fix breaking change to PY_SSIZE_T_CLEAN macro defintion requirements in python >= 3.10
  # by defining the macro ahead of any calls.
  # https://tracker.ceph.com/issues/53441
  # https://github.com/ceph/ceph/pull/44112
  ceph-17.2.0-python-PY_SSIZE_T_CLEAN.patch::https://github.com/ceph/ceph/commit/06e1ef35a8bad958bd735bdd2683e732cf733d86.patch

  # Fix linkage when using system Arrow
  ceph-17.2.0-fix-linkage-with-system-arrow.patch::https://github.com/ceph/ceph/commit/d122ca2fd936eaa242ae1c687f2c6d4767397f2e.patch

  # Fix building with RocksDB 7+
  ceph-17.2.0-rocksdb7-compat.patch::https://github.com/ceph/ceph/commit/eea10281e6f4078f261b05b6bd9c9c9aec129201.patch

  # Fix building with gcc 12+
  ceph-17.2.0-gcc12-missing-memory-include.patch::https://github.com/ceph/ceph/commit/7c381ba985bd1398ef7d145cc00fae9d0db510e3.patch
  ceph-17.2.0-gcc12-FTBFS-due-to-dout-and-need_dynamic.patch::https://github.com/ceph/ceph/commit/963d756ded40f5adf2efef53893c917bec1845c1.patch

  #https://gitweb.gentoo.org/repo/gentoo.git/plain/sys-cluster/ceph/files/ceph-17.2.0-no-virtualenvs.patch
)
sha512sums=('70b96e709f153f16069bec8346ea73812d699eaab91a012351d02addb3fd176b84fce32c3dae84fdf91d0ffe49f2bb258410b47caeee81d81a672b4dbd04cc7e'
            '4354001c1abd9a0c385ba7bd529e3638fb6660b6a88d4e49706d4ac21c81b8e829303a20fb5445730bdac18c4865efb10bc809c1cd56d743c12aa9a52e160049'
            '9e6bb46d5bbdc5d93f4f026b2a8d6bdb692d9ea6e7018c1bb0188d95ea8574c76238d968b340fd67ddaa3d8183b310e393e3549dc3a63a795fde696413b0ca94'
            '6ff46a90d76f667fa23be0f9eb1ed2fb7e30af9a2621aec19944d0a22a431a0f5721603c588286e483ff55c14aac920adfccb039c9678a87cc59640dd70367ae'
            'ea069b75b786c22166c609b127b512802cc5c6e9512d792d7b7b34d276f5b86d57c8c35cfc7b5c855a59c0ba87ba1aabe2ca26da72b26bff46b6ba8410ddb27e'
            '393fb0488dd2652550660684b46d5edb840e966b8b617ad697df75a224444918ce7443c8e74929533131dd6c0761add6896005fd335aa239ebc56a5b55f1c93e'
            '2234d005df71b3b6013e6b76ad07a5791e3af7efec5f41c78eb1a9c92a22a67f0be9560be59b52534e90bfe251bcf32c33d5d40163f3f8f7e7420691f0f4a222'
            'f49427d3420574043e18cad517a6f81ee38a48b827195f564fd728fd2f2b32dbf17f9e21842b01bbf3ab40875c70b0db316a478b8cc48dd92c839d2ce5e7fd63'
            '5a40347785dd1194150d63a7229b68525503969e89fa8867770b1aec3c7a56497930c9ed58cbae346b1a292af350b86340a3041241df9d13433cf2a298e72ec2'
            '9313d18d77d18c76d3d37f8008573031106108f4252054e887aa3b90542247fdb6deceb983defcfbc2650f646d07fc5a97a2fcb0874853f33e0731b3a6d32d00'
            'f1b54767d8d3c12ca9fe9eafd0590efa8560a52b5c18f1121f8fd8b7e69d70578bdeae9a1803612a8a8d0032f039f8786b5152a889ba359850e3d3d30a6af8c7'
            'e54ee5223831b23676f4de7a49fc2548e5deb524ecc0e75a6d4dac1c5e69e9f8fb9bb5fd2e423dd548ad7dd7e3d6c7b4a0e9e68ceaabfa1add8f025687bd4e60')


# -fno-plt causes linker errors (undefined reference to internal methods)
# similar issue: https://bugs.archlinux.org/task/54845
# https://github.com/intel/media-driver/commit/d95d8f7ab7ac94a2e0f4ee6a4b4794898dc2d3b7
# as of today (2019-07-12) the upstream maintainers do not consider this a bug in their code
# (IMHO rightfully so) and thus we strip the option here
export CFLAGS="${CFLAGS/-fno-plt/}"
export CXXFLAGS="${CXXFLAGS/-fno-plt/}"


prepare() {
  cd "${srcdir}/${pkgbase}-${pkgver}"

  # apply patches from the source array
  local filename
  for filename in "${source[@]%%::*}"; do
    if [[ "${filename}" =~ \.patch$ ]]; then
      echo "Applying patch ${filename##*/}"
      patch -p1 -N -i "${srcdir}/${filename##*/}"
    fi
  done

  # temporarily disable unsubscriptable-object (buggy on Python 3.9)
  # https://github.com/PyCQA/pylint/issues/3882
#  sed -i '/^disable=/a\        unsubscriptable-object,' \
#    src/pybind/mgr/dashboard/.pylintrc

  # mypy complains about this but the exception is handled; not sure what's up
  sed -i 's/from base64 import encodestring$/&  # type: ignore/' \
    src/pybind/mgr/dashboard/awsauth.py

  # suppress deprecation warnings
#  sed -i '/#ifndef CEPH_CONFIG_H/i#define BOOST_ALLOW_DEPRECATED_HEADERS' \
#    src/common/config.h
#  sed -i '/#ifndef CEPH_TYPES_H/i#define BOOST_ALLOW_DEPRECATED_HEADERS' \
#    src/include/types.h

  # fix boost stuff for system-boost
#  find . -name '*.cmake' -or -name 'CMakeLists.txt' -print0 | xargs --null \
#    sed -r \
#    -e 's|Boost::|boost_|g' \
#    -e 's|Boost_|boost_|g' \
#    -e 's|[Bb]oost_boost|boost_system|g' -i || exit 1

  # remove tests that require root privileges
  rm src/test/cli/ceph-authtool/cap*.t

  # disable/remove broken tests
  sed -i '/add_ceph_test(smoke.sh/d' src/test/CMakeLists.txt
  sed -i '/add_ceph_test(safe-to-destroy.sh/d' src/test/osd/CMakeLists.txt

  # fix ceph-create-keys hardcoded sbin install location
  sed -i 's/  DESTINATION sbin)/  DESTINATION ${CMAKE_INSTALL_SBINDIR})/' src/CMakeLists.txt
  
  sed -i 's/==.*//' src/mypy-constrains.txt
}

build() {
  cd "${srcdir}/${pkgbase}-${pkgver}"

  # workaround for boost 1.74 -- similar fix exists upstream but I could
  # not get it to work: https://github.com/ceph/ceph/commit/3d708219092d
#  CPPFLAGS+=' -DBOOST_ASIO_USE_TS_EXECUTOR_AS_DEFAULT' # https://github.com/ceph/ceph/commit/7ce3ee6f346889d4d87d6424c6a1ad18badd139b

  export CFLAGS+=" ${CPPFLAGS}"
  export CXXFLAGS+=" ${CPPFLAGS}"
  export PYTHON_VERSION="$(python --version | awk '{print $2}')"
#  export PYTHON_INCLUDE_DIR="$(python -c "from sysconfig import get_path; print(get_path('include'))")"
  export CMAKE_BUILD_TYPE='RelWithDebInfo'
#  export CMAKE_WARN_UNUSED_CLI=no

  cmake \
    -B build \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_INSTALL_SYSCONFDIR=/etc \
    -DCMAKE_INSTALL_SBINDIR=/usr/bin \
    -DCMAKE_INSTALL_LIBDIR=/usr/lib \
    -DCMAKE_INSTALL_LIBEXECDIR=/usr/lib \
    -DCEPH_SYSTEMD_ENV_DIR=/etc/conf.d \
    -DSYSTEMD_SYSTEM_UNIT_DIR=/usr/lib/systemd/system \
    -DCMAKE_VERBOSE_MAKEFILE=ON \
    -DENABLE_GIT_VERSION=ON \
    -DWITH_PYTHON3="${PYTHON_VERSION}" \
    -DWITH_BABELTRACE=OFF \
    -DWITH_LTTNG=OFF \
    -DWITH_OPENLDAP=OFF \
    -DWITH_RDMA=OFF \
    -DWITH_OCF=OFF \
    -DWITH_DPDK=OFF \
    -DWITH_SPDK=OFF \
    -DWITH_CEPHFS=ON \
    -DWITH_CEPHFS_JAVA=ON \
    -DWITH_CEPHFS_SHELL=ON \
    -DWITH_FUSE=ON \
    -DWITH_LZ4=ON \
    -DWITH_XFS=ON \
    -DWITH_MGR=ON \
    -DWITH_MGR_DASHBOARD_FRONTEND=ON \
    -DDASHBOARD_FRONTEND_LANGS="ALL" \
    -DWITH_RADOSGW=ON \
    -DWITH_RADOSGW_BEAST_OPENSSL=ON \
    -DWITH_RADOSGW_AMQP_ENDPOINT=OFF \
    -DWITH_RADOSGW_SELECT_PARQUET=ON \
    -DWITH_SYSTEMD=ON \
    -DWITH_SYSTEM_BOOST=ON \
    -DWITH_SYSTEM_ARROW=ON \
    -DWITH_SYSTEM_GTEST=OFF \
    -DWITH_SYSTEM_LIBURING=ON \
    -DWITH_SYSTEM_NPM=OFF \
    -DWITH_SYSTEM_ROCKSDB=ON \
    -DWITH_SYSTEM_ZSTD=ON \
    -DENABLE_SHARED=ON \
    -DWITH_TESTS=OFF \
    -Wno-dev
#    -DWITH_PYTHON2=OFF \ # https://github.com/ceph/ceph/commit/583482829b8537efc1241c998716161cdedb580e
#    -DPYTHON_INCLUDE_DIR="${PYTHON_INCLUDE_DIR:?}" \
#    -DMGR_PYTHON_VERSION=3 \ # only used by source builds of boost and arrow
#    -DWITH_BOOST_CONTEXT=ON \ # https://github.com/ceph/ceph/commit/b132d689301d5861ea784b975052177ae29c4cf4
#    -DWITH_RADOSGW_BEAST_FRONTEND=ON \ # https://github.com/ceph/ceph/commit/bf33951219e42271469f64e8cffa68d9628ef826
#    -DWITH_RADOSGW_FCGI_FRONTEND=OFF \ # https://github.com/ceph/ceph/commit/a3353040423f91bfc700566be30476aedfed2198

  make -C build legacy-option-headers # avoid a race in job ordering
  VERBOSE=1 make -C build all
}

###
### testsuite currently broken, needs some debugging
###
# check() {
#   cd "${srcdir}/${pkgbase}-${pkgver}"
# 
#   export CTEST_PARALLEL_LEVEL=8
#   export CTEST_OUTPUT_ON_FAILURE=1
#   VERBOSE=1 make -C build check
# 
#   # sometimes processes are not properly terminated...
#   for process in ceph-mon ceph-mgr ceph-osd; do
#     pkill -9 "${process}" || true
#   done
# }

package_ceph-libs() {
  depends=('arrow' 'boost-libs' 'bzip2' 'cryptsetup' 'curl' 'fmt' 'glibc'
           'keyutils' 'liburing' 'libutil-linux' 'lua' 'lz4'
           'oath-toolkit' 'python' 'rocksdb' 'snappy' 'sqlite' 'systemd-libs'
           "zstd=${_zstdver}")

  cd "${srcdir}/${pkgbase}-${pkgver}"

  # main install
  VERBOSE=1 make DESTDIR="${pkgdir}" -C build install

  # remove stuff that goes into the ceph package
  rm -rf "${pkgdir}"/usr/lib/{ceph/mgr,systemd,sysusers.d,tmpfiles.d}
  rm -rf "${pkgdir}/usr/share"
#  rm -rf "${pkgdir}/usr/sbin"
  rm -rf "${pkgdir}/usr/bin"
  rm -rf "${pkgdir}/etc"
  rm -rf "${pkgdir}/var"
}

package_ceph() {
  depends=("ceph-libs=${pkgver}-${pkgrel}"
           'boost-libs' 'curl' 'fuse3' 'fmt' 'glibc' 'gperftools'
           'java-runtime' 'keyutils' 'libaio' 'libnl' 'librdkafka'
           'libutil-linux' 'ncurses' 'oath-toolkit' 'python' 'python-bcrypt'
           'python-cmd2' 'python-dateutil' 'python-flask' 'python-pecan'
           'python-prettytable' 'python-pyaml' 'python-pyopenssl'
           'python-requests' 'python-setuptools' 'python-werkzeug'
           'python-yaml' 'snappy' 'sudo' 'systemd-libs' 'xfsprogs')

  cd "${srcdir}/${pkgbase}-${pkgver}"

  # main install
  VERBOSE=1 make DESTDIR="${pkgdir}" -C build install

  # fix sbin dir (cmake opt seems to have no effect)
#  mv "${pkgdir}"/usr/sbin/* "${pkgdir}/usr/bin/"
#  rm -rf "${pkgdir}/usr/sbin"

  # remove stuff that is in the ceph-libs package
  find "${pkgdir}/usr/lib" -maxdepth 1 -type f -delete
  find "${pkgdir}/usr/lib" -maxdepth 1 -type l -delete
  find "${pkgdir}/usr/lib/ceph" -maxdepth 1 -type f -delete
  find "${pkgdir}/usr/lib/ceph" -maxdepth 1 -type l -delete
  find "${pkgdir}/usr/lib/ceph/denc" -maxdepth 1 -type f -delete
  find "${pkgdir}/usr/lib/ceph/librbd" -maxdepth 1 -type f -delete
  find "${pkgdir}/usr/lib/ceph/librbd" -maxdepth 1 -type l -delete
  rm -rf "${pkgdir}"/usr/lib/{ceph/{compressor,crypto,erasure-code},rados-classes}
  rm -rf "${pkgdir}"/usr/lib/python*
  rm -rf "${pkgdir}/usr/include"

  # remove stuff that is in the ceph-mgr package
  rm -rf "${pkgdir}"/usr/{bin/ceph-mgr,share/ceph/mgr,lib/systemd/system/ceph-mgr*}

  # remove _test_ binaries from the package, not needed
  find "${pkgdir}/usr/bin" -maxdepth 1 -type f -iname 'ceph_test_*' -delete

  # install tmpfiles.d and sysusers.d stuff
  install -Dm644 "${srcdir}/${pkgbase}-${pkgver}/systemd/ceph.tmpfiles.d" \
    "${pkgdir}/usr/lib/tmpfiles.d/${pkgbase}.conf"
  install -Dm644 "${srcdir}/ceph.sysusers" \
    "${pkgdir}/usr/lib/sysusers.d/${pkgbase}.conf"

  # remove debian init script
  # rm -rf "${pkgdir}/etc/init.d"

  # remove drop.ceph.com ssh stuff
  rm -f "${pkgdir}"/usr/share/ceph/id_rsa_drop.ceph.com
  rm -f "${pkgdir}"/usr/share/ceph/id_rsa_drop.ceph.com.pub
  rm -f "${pkgdir}"/usr/share/ceph/known_hosts_drop.ceph.com

  # fix bash completions path
  install -d -m 755 "${pkgdir}/usr/share/bash-completion"
  mv "${pkgdir}"/{etc/bash_completion.d,usr/share/bash-completion/completions}

  # fix EnvironmentFile location in systemd service files
#  sed -i 's|/etc/sysconfig/|/etc/conf.d/|g' "${pkgdir}"/usr/lib/systemd/system/*.service

  # prepare some paths and set correct permissions
  install -D -d -m750 -o   0 -g 340 "${pkgdir}/etc/ceph"
  install -D -d -m750 -o 340 -g 340 "${pkgdir}/var/log/ceph"
  install -D -d -m750 -o 340 -g 340 "${pkgdir}/var/lib/ceph"
  install -D -d -m750 -o 340 -g 340 "${pkgdir}/var/lib/ceph/bootstrap-mds"
  install -D -d -m750 -o 340 -g 340 "${pkgdir}/var/lib/ceph/bootstrap-osd"
  install -D -d -m750 -o 340 -g 340 "${pkgdir}/var/lib/ceph/bootstrap-rgw"
  install -D -d -m750 -o 340 -g 340 "${pkgdir}/var/lib/ceph/mon"
  install -D -d -m750 -o 340 -g 340 "${pkgdir}/var/lib/ceph/osd"
}

package_ceph-mgr() {
  depends=("ceph=${pkgver}-${pkgrel}" "ceph-libs=${pkgver}-${pkgrel}"
           'bash' 'boost-libs' 'coffeescript' 'curl' 'gperftools' 'nodejs'
           'python' 'python-cherrypy' 'python-flask' 'python-jsonpatch'
           'python-more-itertools'  'python-numpy' 'python-pecan'
           'python-pyjwt' 'python-routes' 'python-scipy')
  optdepends=('python-influxdb: influx module'
              'python-kubernetes: rook module'
              'python-prometheus_client: prometheus module'
              'python-remoto: ssh module')
  conflicts=('ceph<14.2.1-1')

  cd "${srcdir}/${pkgbase}-${pkgver}"

  # main install
  VERBOSE=1 make DESTDIR="${pkgdir}" -C build install

  # fix sbin dir (cmake opt seems to have no effect)
#  mv "${pkgdir}"/usr/sbin/* "${pkgdir}/usr/bin/"
#  rm -rf "${pkgdir}/usr/sbin"

  # remove everything except mgr related stuff, rest is in ceph/ceph-libs
  rm -rf "${pkgdir}"/usr/lib/{ceph/{compressor,crypto,erasure-code},rados-classes}
  rm -rf "${pkgdir}/usr/include"
  find "${pkgdir}/usr/bin" -maxdepth 1 -type f -not -name 'ceph-mgr' -delete
  find "${pkgdir}"/usr/lib/systemd/system -maxdepth 1 -type f -not -iname 'ceph-mgr*' -delete
  find "${pkgdir}"/usr/lib -maxdepth 1 -type f -delete
  find "${pkgdir}"/usr/lib -maxdepth 1 -type l -delete
  rm -rf "${pkgdir}"/etc
  rm -rf "${pkgdir}"/var
  rm -rf "${pkgdir}"/usr/lib/{ceph,sysusers.d,tmpfiles.d}
  rm -rf "${pkgdir}"/usr/lib/python*
  rm -rf "${pkgdir}"/usr/share/{bash-completion,doc,java,man}

  # remove debian init script
  # rm -rf "${pkgdir}/etc/init.d"

  # remove drop.ceph.com ssh stuff
  rm -f "${pkgdir}"/usr/share/ceph/id_rsa_drop.ceph.com
  rm -f "${pkgdir}"/usr/share/ceph/id_rsa_drop.ceph.com.pub
  rm -f "${pkgdir}"/usr/share/ceph/known_hosts_drop.ceph.com

  # fix EnvironmentFile location in systemd service files
#  sed -i 's|/etc/sysconfig/|/etc/conf.d/|g' "${pkgdir}"/usr/lib/systemd/system/*.service

  # prepare some paths and set correct permissions
  install -D -d -m750 -o 340 -g 340 "${pkgdir}/var/lib/ceph/mgr"
}

# vim:set ts=2 sw=2 et:
