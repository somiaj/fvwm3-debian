# fvwm3-debian

This is the `debian/` directory for fvwm3 to build a
Debian package (.deb) from the fvwm3 source.

This is meant to build a fvwm3 package from the current master
branch from the [fvwm3](https://github.com/fvwmorg/fvwm3)
repository. The official release (1.0.3 at time of writing)
will be added to Debian (it is sitting in New at time of writing).

## Use

Build the current [fvwm3](https://github.com/fvwmorg/fvwm3)
source and package it in a Debian package for bullseye as follows.

Adjust to suit your needs.

+ Clone fvwm3 and this repo.

  ```
  git clone https://github.com/fvwmorg/fvwm3.git
  git clone https://github.com/somiaj/fvwm3-debian.git
  ```

+ Install the Debian build tools and build dependencies.

  ```
  apt install build-essential debhelper asciidoctor fontconfig gettext libbson-dev \
              libevent-dev libfontconfig-dev libfreetype6-dev libfribidi-dev \
              libncurses-dev libpng-dev libreadline-dev librsvg2-dev \
              libsm-dev libx11-dev libxcursor-dev libxext-dev libxft-dev \
              libxi-dev libxpm-dev libxrandr-dev libxrender-dev libxt-dev
  ```

  If you want to build for buster, you need to install
  debhelper from backports. Provided you have backports
  enabled:

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
  cd ..
  sudo apt install ./fvwm3_1.0.4-1~rc1_amd64.deb
  ```


