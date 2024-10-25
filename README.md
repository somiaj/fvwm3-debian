# fvwm3-debian

This is the `debian/` directory for fvwm3 to build a
Debian package (.deb) from the fvwm3 source.

+ Starting with fvwm3 1.1.5 the Debian package now builds
  `FvwmPrompt` using Debian's golang libraries, and no
  loner builds `FvwmConsole`. Config files will need to be
  updated, see `debian/NEWS`.
  
+ **Build Note:** Starting with fvwm3 1.1.1 the build system
  has moved from autotools to meson (version 1.5.1 or greater).
  The Debian package now builds using meson. The old `debian/`
  directory to build with autotools can be found in the
  `autotools` branch.

+ This only contains the `debian/` directory. Get the upstream
  [fvwm3 source on github](https://github.com/fvwmorg/fvwm3).

+ The `main` branch is used to build against the `main` fvwm3 branch,
  which reflects the current development status of fvwm3.

+ Branches to build specific versions from released tarballs are
  the same as the release number. For example branch `1.1.0` will
  build against the [1.1.0 release of fvwm3](
  https://github.com/fvwmorg/fvwm3/releases/tag/1.1.0).

+ This builds a package that can be installed along side the fvwm
  package (which is fvwm version 2). To do this some binaries and
  manual pages have been renamed. Check `debian/README.Debian` for
  details.

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
  apt install build-essential debhelper dh-builtusing dh-golang asciidoctor \
              fontconfig gettext golang-any golang-github-ergochat-readline-dev \
              golang-github-fatih-color-dev golang-github-mattn-go-colorable-dev \
              golang-github-mattn-go-isatty-dev golang-github-sirupsen-logrus-dev \
              golang-golang-x-sys-dev golang-golang-x-text-dev libevent-dev \
              libfontconfig-dev libfreetype6-dev libfribidi-dev libncurses-dev \
              libpng-dev libreadline-dev librsvg2-dev libsm-dev libx11-dev \
              libxcursor-dev libxext-dev libxft-dev libxi-dev libxkbcommon-dev \
              libxpm-dev libxrandr-dev libxrender-dev libxt-dev meson
  ```

+ Copy `debian/` into the fvwm3 source and build the package.

  ```
  cd fvwm3
  cp -r ../fvwm3-debian/debian .
  dpkg-buildpackage -us -uc -b
  ```

  Note, you can also use a symlink, `ln ../fvwm3-debian/debian/ .` as well.

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
  meson setup build -Dgolang=enabled -Dhtmldoc=true -Dmandoc=true
  meson dist -C build
  mk-origtargz build/meson-dist/fvwm3-<version>.tar.xz
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

