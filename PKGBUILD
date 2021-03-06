# Maintainer: Timon Engelke <aur@timonengelke.de>
pkgdesc="ROS - Enhanced tools for benchmarks in MoveIt!."
url='https://moveit.ros.org'

pkgname='ros-melodic-moveit-ros-benchmarks'
pkgver='1.0.3'
arch=('i686' 'x86_64' 'aarch64' 'armv7h' 'armv6h')
pkgrel=2
license=('BSD')

ros_makedepends=(ros-melodic-pluginlib
  ros-melodic-roscpp
  ros-melodic-moveit-ros-planning
  ros-melodic-moveit-ros-warehouse
  ros-melodic-moveit-core
  ros-melodic-catkin)
makedepends=('cmake' 'ros-build-tools'
  ${ros_makedepends[@]})

ros_depends=(ros-melodic-pluginlib
  ros-melodic-roscpp
  ros-melodic-moveit-ros-planning
  ros-melodic-moveit-ros-warehouse
  ros-melodic-moveit-core)
depends=(${ros_depends[@]})

# Git version (e.g. for debugging)
# _tag=release/melodic/moveit_ros_benchmarks/${pkgver}-${_pkgver_patch}
# _dir=${pkgname}
# source=("${_dir}"::"git+https://github.com/ros-gbp/moveit-release.git"#tag=${_tag})
# sha256sums=('c84005b45be5707a4e6d5a01c966a9a13ea5c8f754c8c35b5d3fdb2d2ed44c56')

# Tarball version (faster download)
_dir="moveit-${pkgver}/moveit_ros_benchmarks"
source=("${pkgname}-${pkgver}.tar.gz"::"https://github.com/ros-planning/moveit/archive/${pkgver}.tar.gz")
sha256sums=('b0ac91cd4c4dc29d9bd5e3885a1a457252495b3f2bedb46ddfe04154f5ac2358')

build() {
  # Use ROS environment variables
  source /usr/share/ros-build-tools/clear-ros-env.sh
  [ -f /opt/ros/melodic/setup.bash ] && source /opt/ros/melodic/setup.bash

  # Create build directory
  [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  cd ${srcdir}/build

  # Fix Python2/Python3 conflicts
  /usr/share/ros-build-tools/fix-python-scripts.sh -v 3 ${srcdir}/${_dir}

  # Build project
  cmake ${srcdir}/${_dir} \
        -DCMAKE_BUILD_TYPE=Release \
        -DCATKIN_BUILD_BINARY_PACKAGE=ON \
        -DCMAKE_INSTALL_PREFIX=/opt/ros/melodic \
        -DPYTHON_EXECUTABLE=/usr/bin/python3 \
        -DSETUPTOOLS_DEB_LAYOUT=OFF
  make
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}/" install
}
