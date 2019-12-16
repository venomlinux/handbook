# Writing ports

Venom Linux use `port-like` recipe to create packages. `pkgbuild` and `spkgbuild` is used to create
packages. `pkgbuild` is a bash script that sourcing `spkgbuild` before creating packages. `spkgbuild`
a recipe contains all information and build command on how to build packages.

In this guide i will use `dfc` as example. `dfc` is a commandline tool to displays file system space
usage using graphs and colors. `dfc` website is https://projects.gw-computing.net/projects/dfc and
the source tarball is https://projects.gw-computing.net/attachments/download/615/dfc-3.1.1.tar.gz got
from the website.

## Requirement

- fakeroot
- scratchpkg-utils

> Note: Install it using scratch if not installed yet.

## Create local repository

I'm suggesting you to create a local repository to store all of your own ports. I'm gonna create
local repository in `$HOME` and `cd` into it.

```
$ mkdir myports
$ cd myports
$ pwd
/home/emmett/myports
```

## Create ports template

Theres a tool called `portcreate` in `scratchpkg-utils` package. Use it to create initial
template for your port then `cd` into its directory. The usage is `portcreate <pkg>`.

```
$ portcreate dfc
Template port have created for 'dfc'.
$ cd dfc
$ pwd
/home/emmett/myports/dfc
```

The template created as follows:

```
# description   :
# homepage      :
# depends       :

name=dfc
version=
release=1
options=()
noextract=()
backup=()
source=()
md5sum=()

build() {
        cd $name-$version
        ./configure --prefix=/usr
        make
        make DESTDIR=$PKG install
}
```

## Edit spkgbuild

Edit `spkgbuild` to insert needed variable, information and build commands. Use your favourite
text editor to edit it.

```
$ vim spkgbuild
```

Heres is the result of `dfc`'s `spkgbuild`:

```
# description	: Commandline tool to displays file system space usage using graphs and colors
# homepage	: https://projects.gw-computing.net/projects/dfc
# depends	: cmake

name=dfc
version=3.1.1
release=1
source=(https://projects.gw-computing.net/attachments/download/615/dfc-$version.tar.gz)
md5sum=()

build() {
	cd $name-$version

	cmake . -DPREFIX=/usr \
		-DSYSCONFDIR=/etc \
		-DCMAKE_BUILD_TYPE=RELEASE
        make
        make DESTDIR=$PKG install

	# remove unused stuff
        rm -r $PKG/usr/share/{doc,locale} $PKG/{usr/share/man,etc/xdg/dfc}/{fr,nl}
}
```

Notice theres still empty on `md5sum=()` array. So we gonna use `pkgbuild` to generate
md5sum. First run `pkgbuild` to fetch sources.

```
$ pkgbuild
==> Downloading 'https://projects.gw-computing.net/attachments/download/615/dfc-3.1.1.tar.gz'.
--2019-12-16 14:11:54--  https://projects.gw-computing.net/attachments/download/615/dfc-3.1.1.tar.gz
Resolving projects.gw-computing.net (projects.gw-computing.net)... 37.59.30.58
Connecting to projects.gw-computing.net (projects.gw-computing.net)|37.59.30.58|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [application/gzip]
Saving to: '/mnt/data/venom/sources/dfc-3.1.1.tar.gz.partial'

/mnt/data/venom/sources/dfc-     [   <=>                                        ]  51.47K  45.0KB/s    in 1.1s

2019-12-16 14:11:58 (45.0 KB/s) - '/mnt/data/venom/sources/dfc-3.1.1.tar.gz.partial' saved [52709]

==> ERROR: md5sum=() is empty, please provide it.
```

After fetch source is successful, then run `pkgbuild -g` to generate md5sum.

```
$ pkgbuild -g
md5sum=(26fd905a07078332d98c2806cdd0fc0e)
```

After md5sum is generated, insert it to `md5sum=()` array in `spkgbuild`. Ok now spkgbuild is
complete. Its time to build package. Run `fakeroot pkgbuild` to build it.

> Note: `pkgbuild` required root access to build package to get right root id for files in package
> But for new port, its recommend using `fakeroot` to build it, incase something is wrong in
> `spkgbuild`, it will not harm your system.

```
$ fakeroot pkgbuild
==> Source 'dfc-3.1.1.tar.gz' found.
 -> Unpacking 'dfc-3.1.1.tar.gz'...
==> Start build 'dfc-3.1.1-1'.
+ build
+ cd dfc-3.1.1
+ cmake . -DPREFIX=/usr -DSYSCONFDIR=/etc -DCMAKE_BUILD_TYPE=RELEASE
CMake Warning (dev) at /usr/share/cmake-3.16/Modules/Compiler/ADSP-DetermineCompiler.cmake:4 (set):
  Policy CMP0053 is not set: Simplify variable reference and escape sequence
  evaluation.  Run "cmake --help-policy CMP0053" for policy details.  Use the
  cmake_policy command to set the policy and suppress this warning.

  For input:

    '
    #if defined(__VISUALDSPVERSION__)
      /* __VISUALDSPVERSION__ = 0xVVRRPP00 */
    # define @PREFIX@COMPILER_VERSION_MAJOR @MACRO_HEX@(__VISUALDSPVERSION__>>24)
    # define @PREFIX@COMPILER_VERSION_MINOR @MACRO_HEX@(__VISUALDSPVERSION__>>16 & 0xFF)
...
...
...
-- Installing: /mnt/data/venom/work/dfc/pkg/usr/share/doc/dfc/README.md
-- Installing: /mnt/data/venom/work/dfc/pkg/usr/share/doc/dfc/TRANSLATORS.md
-- Installing: /mnt/data/venom/work/dfc/pkg/usr/share/locale/fr/LC_MESSAGES/dfc.mo
-- Installing: /mnt/data/venom/work/dfc/pkg/usr/share/locale/nl/LC_MESSAGES/dfc.mo
+ rm -r /mnt/data/venom/work/dfc/pkg/usr/share/doc /mnt/data/venom/work/dfc/pkg/usr/share/locale /mnt/data/venom/work/dfc/pkg/usr/share/man/fr /mnt/data/venom/work/dfc/pkg/usr/share/man/nl /mnt/data/venom/work/dfc/pkg/etc/xdg/dfc/fr /mnt/data/venom/work/dfc/pkg/etc/xdg/dfc/nl
==> Build 'dfc-3.1.1-1' success.
drwxrwxr-x root/root         0 2019-12-16 14:19 etc/
drwxrwxr-x root/root         0 2019-12-16 14:19 etc/xdg/
drwxrwxr-x root/root         0 2019-12-16 14:19 etc/xdg/dfc/
-rw-r--r-- root/root      1672 2017-09-09 15:11 etc/xdg/dfc/dfcrc
drwxrwxr-x root/root         0 2019-12-16 14:19 usr/
drwxrwxr-x root/root         0 2019-12-16 14:19 usr/share/
drwxrwxr-x root/root         0 2019-12-16 14:19 usr/share/man/
drwxrwxr-x root/root         0 2019-12-16 14:19 usr/share/man/man1/
-rw-r--r-- root/root      2473 2017-09-09 15:11 usr/share/man/man1/dfc.1.gz
drwxrwxr-x root/root         0 2019-12-16 14:19 usr/bin/
-rwxr-xr-x root/root     51984 2019-12-16 14:19 usr/bin/dfc
==> Successfully created package 'dfc-3.1.1-1.spkg.tar.xz'. (21K)
```

If nothing goes wrong, you will succeed build the package. Then you can install it using `pkgbuild -i`. This time
you really need root to install to real system, use sudo.

```
$ sudo pkgbuild
```

Also you can install using `pkgadd <pkg.spkg.tar.xz>`. By default, fetched source will be in
`/var/cache/scratchpkg/sources` and created package will be in `/var/cache/scratchpkg/packages`
directory. You can modify this location in `scratchpkg` config file, `/etc/scratchpkg.conf`.
