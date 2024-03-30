# fvwm3-debian

This is the `debian/` directory for fvwm3 to build a
Debian package (.deb) from the fvwm3 source.

The main branch is used to build a fvwm3 package for the 
[1.1.0 release of fvwm3](https://github.com/fvwmorg/fvwm3/releases/tag/1.1.0).
To build against the current fvwm3 master branch, use the fvwm3-git branch.

This builds a package that can be installed along side the fvwm package
(which is fvwm version 2). To do this some binaries and manual pages
have been renamed. Check `debian/NEWS` for details.

## Build Instructions for Git

These instructions are to build an fvwm3 package from git.
Adjust to suit your needs.

+ Clone fvwm3 and this repo.

  ```
  git clone https://github.com/fvwmorg/fvwm3.git
  git clone https://github.com/somiaj/fvwm3-debian.git
  ```

+ Switch to the fvwm3-git branch.

  ```
  cd fvwm3-debian
  git checkout fvwm3-git
  cd ..
  ```

+ Install the Debian build tools and build dependencies.

  ```
  apt install build-essential debhelper asciidoctor fontconfig gettext \
              libevent-dev libfontconfig-dev libfreetype6-dev libfribidi-dev \
              libncurses-dev libpng-dev libreadline-dev librsvg2-dev \
              libsm-dev libx11-dev libxcursor-dev libxext-dev libxft-dev \
              libxi-dev libxpm-dev libxrandr-dev libxrender-dev libxt-dev
  ```

+ Copy `debian/` into the fvwm3 source and build the package.

  ```
  cd fvwm3
  cp -r ../fvwm3-debian/debian ./
  dpkg-buildpackage -us -uc -b
  ```

+ Install the resulting .deb package.

  ```
  sudo apt install ../fvwm3_1.1.*_amd64.deb
  ```

## FvwmPrompt

Due to Debian policy, the official Debian package cannot contain bundled
libraries, so `bin/FvwmPrompt/vendor` is removed from the Debian source
package, and FvwmPrompt must be built using Debian system libraries.

Currently Debian is missing a few golang depends to build FvwmPrompt,
see `golang-depends.md`, so the Debian package won't contain FvwmPrompt
until all the depends are packaged for Debian.

To build a local Debian package that uses `bin/FvwmPrompt/vendor`
to build FvwmPrompt, use the official fvwm3 source. Then
use the patch in the branch `FvwmPrompt` to build a package that
also builds FvwmPrompt. Use the above instructions along with
installing the package `golang-go` to build the package.

