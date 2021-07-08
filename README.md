# fvwm3-debian

This is the `debian/` directory for fvwm3 to build a
Debian package (.deb) from the fvwm3 source.

The main branch is to build a fvwm3 package from the current master
branch of the [fvwm3](https://github.com/fvwmorg/fvwm3) repository.
Branches will be used to create snapshots to build a specific fvwm3
release version.

This builds a package that can be installed along side the fvwm package
(which is fvwm version 2). To do this some binaries and manual pages
have been renamed. Check `debian/NEWS` for details.

## Build Instructions

Build the current [fvwm3](https://github.com/fvwmorg/fvwm3) source
and package it in a Debian package for bullseye (or buster) as follows.

Adjust to suit your needs.

+ Clone fvwm3 and this repo.

  ```
  git clone https://github.com/fvwmorg/fvwm3.git
  git clone https://github.com/somiaj/fvwm3-debian.git
  ```

+ Install the Debian build tools and build dependencies.

  ```
  apt install build-essential debhelper asciidoctor fontconfig gettext \
              libevent-dev libfontconfig-dev libfreetype6-dev libfribidi-dev \
              libncurses-dev libpng-dev libreadline-dev librsvg2-dev \
              libsm-dev libx11-dev libxcursor-dev libxext-dev libxft-dev \
              libxi-dev libxpm-dev libxrandr-dev libxrender-dev libxt-dev
  ```

  If building for buster, install debhelper from `buster-backports`.
  Provided you have `buster-backports` in your sources:

  ```
  apt -t buster-backports install debhelper
  ```
+ Copy `debian/` into the fvwm3 source and build the package.

  ```
  cd fvwm3
  cp -r ../fvwm3-debian/debian ./
  dpkg-buildpackage -us -uc -b
  ```

+ Install the resulting .deb package.

  ```
  sudo apt install ../fvwm3_1.0.4-1~rc1_amd64.deb
  ```


