# scratchpkg

`scratchpkg` is a simple package manager for Venom Linux for managing packages installed. 
`scratchpkg` comes with a some main tools, `scratch`, `pkgbuild`, `pkgadd`, `pkgdel`, `revdep`
and `updateconf`.

## scratch

`scratch` is a tool fronted for `pkgbuild`, `pkgadd` and `pkgdel`. `scratch` can use to install,
remove, upgrade, observe installed packages and etc. Heres quick usage for `scratch`:

Search for package in repo:
```
$ scratch search <pattern>
```

> Note: scratch will search packages match on package's name and description.

Update repo:
```
# scratch sync
```

Make full system upgrade:
```
# scratch sysup
```

Check for outdated packages:
```
# scratch outdate
```
> Note: You need to sync repo first to get list outdated packages and make
> full system upgrade.

Installing package(s):
```
# scratch install pkg1 pkg2 pkg3 pkgN ...
```

Upgrading single/multiple package(s):
```
# scratch upgrade pkg1 pkg2 pkg3 pkgN ...
```

> Note: When upgrading package(s), new dependencies will be installed.

# Removing package(s)
```
# scratch remove pkg1 pkg2 pkg3 pkgN ...
```

For other function just run:
```
$ scratch help
```

## pkgbuild

`pkgbuild` is a tool to build a package from package's recipe (called port). 
For normal user, `scratch` will take care of building package(s).

## pkgadd

`pkgbuild` is a tool to installing precompiled package. The usage is:
```
# pkgadd <pkg>.spkg.tar.xz
```

## pkgdel

`pkgdel` is a tool to remove installed package from system. The usage is:
```
# pkgdel <pkg>
```

## revdep

`revdep` is a script to find and fix (rebuild and reinstall pkgs) broken shared library
linkage. This script is recommended to run after **upgrading** and **removing** packages.

## updateconf

`updateconf` is a tool to update configuration files in `/etc`. This script is recommended
to run after **upgrading** package(s).

> NOTE: For all tools, run `<tool> -h` or `<tool> --help` to get usage help.
> Except for `scratch`, run `scratch help` to get usage help.
