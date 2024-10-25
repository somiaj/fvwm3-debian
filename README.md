# fvwm3-debian

This is the `debian/` directory for fvwm3 to build a
Debian package (.deb) from the fvwm3 source.

+ This only contains the `debian/` directory. Get the upstream
  [fvwm3 source on github](https://github.com/fvwmorg/fvwm3).

+ The `main` branch is used to build against the `main` fvwm3 branch,
  which reflects the current development status of fvwm3.

+ Branches to build specific versions from released tarballs are
  the same as the release number. For example branch `1.1.0` will
  build against the [1.1.0 release of fvwm3](
  https://github.com/fvwmorg/fvwm3/releases/tag/1.1.0).

+ The `FvwmPrompt` branch has a single patch on top of the main
  branch to build `FvwmPrompt` (see below).

+ This builds a package that can be installed along side the fvwm
  package (which is fvwm version 2). To do this some binaries and
  manual pages have been renamed. Check `debian/NEWS` for details.

+ The version of the package appends `ds` (Debian source) to the
  upstream version to indicate that the official Debian package
  removes the bundled libraries, `bin/FvwmPrompt/vendor`, due to
  Debian policy.

## Build instructions for git

These instructions are to build an fvwm3 binary package from git.
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
              libxi-dev libxkbcommon-dev libxpm-dev libxrandr-dev \
              libxrender-dev libxt-dev
  ```

+ Copy `debian/` into the fvwm3 source and build the package.

  ```
  cd fvwm3
  cp -r ../fvwm3-debian/debian .
  dpkg-buildpackage -us -uc -b
  ```

  Note, you can also use a link, `ln ../fvwm3-debian/debian/ .` as well.

+ Install the resulting .deb package.

  ```
  sudo apt install ../fvwm3_1.1.*_amd64.deb
  ```

## Build Debian source package from git.

Use the following to build a Debian source package along side the
binary package. In short this adds an extra step of creating an
original tarball of the source with `bin/FvwmPrompt/vendor` removed.
This isn't needed if you just want to install fvwm3 from a Debian package.

+ Crate a clone of both `fvwm3` and `fvwm3-debian`, copy the `debian/`
  directory from `fvwm3-debain` to `fvwm3`, and install the build-depends
  as described above.

+ Create an original tarball of the current source. This requires
  first installing the `devscripts` package for `mk-origtargz`.
  Note `<version>` will be the next fvwm3 version.

  ```
  cd fvwm3/
  ./autogen.sh
  ./configure
  make dist
  mk-origtargz fvwm3-<version>.tar.gz
  cd ..
  ```

+ Extract the now modified original tarball to build from, and copy
  the `debian/` directory into the extracted source, and build both
  the source and binary package.

  ```
  tar xvf fvwm3_<version>+ds.orig.tar.xz
  cp -r fvwm3-debian/debian/ fvwm3-<version>
  cd fvwm3-<version>
  dpkg-buildpackage -us -uc
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
also builds FvwmPrompt. Use the above instructions for building
a binary package along with checking out the FvwmPrompt branch,
`git checkout FvwmPrompt`, and installing the package `golang-go`
to build the package.

