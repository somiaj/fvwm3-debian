# fvwm3-debian

This is the debian/ directory for fvwm3 to build a
Debian package (.deb) from the fvwm3 source.

## Use

Build the current [fvwm3](https://github.com/fvwmorg/fvwm3)
source and package it in a Debian package for bullseye as follows.

Adjust to suit your needs.

+ Clone fvwm3 and this repo.

  ```
  $ git clone https://github.com/fvwmorg/fvwm3.git
  $ git clone https://github.com/somiaj/fvwm3-debian.git
  ```

+ Install the Debian build tools and build dependencies
  (this assumes you have a deb-src in your sources.list).

  ```
  $ apt install build-essential
  $ apt build-dep fvwm
  $ apt install libbson-dev libevent-dev
  ```

  If you want to build for buster, you need to install
  debhelper from backports. Provided you have backports
  enabled:

  ```
  $ apt -t backports install debhelper
  ```
+ Copy the debian/ into the fvwm3 source and build the package.

  ```
  $ cd fvwm3
  $ cp -r ../fvwm3-debian/debian ./
  $ dpkg-buildpackage -us -uc -b
  ```

+ Install the resulting .deb package.

  ```
  $ cd ..
  $ sudo dpkg -i fvwm3_1.0.3-1_amd64.deb
  ```


