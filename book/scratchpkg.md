# scratchpkg

`scratchpkg` is a simple package manager for Venom Linux. 
`scratchpkg` comes with the following tools: `scratch`, `pkgbuild`, `pkgadd`, `pkgdel`, `revdep`
and `updateconf`.

## scratch

`scratch` is a frontend for `pkgbuild`, `pkgadd` and `pkgdel`. `scratch` can install,
remove, upgrade, query installed packages, etc.

Search for a package in the repositories:
```
$ scratch search <pattern>
```

> Note: scratch will search for packages matching package names and descriptions.

Sync repositories:
```
# scratch sync
```

Perform a full system upgrade:
```
# scratch sysup
```

Check for outdated packages:
```
# scratch outdate
```
> Note: You may need to sync the repositories first, to check for outdated packages.

Installing package(s):
```
# scratch install pkg1 pkg2 pkg3 pkgN ...
```

Upgrading package(s):
```
# scratch upgrade pkg1 pkg2 pkg3 pkgN ...
```

> Note: When upgrading packages, new dependencies may be installed.

Removing package(s):
```
# scratch remove pkg1 pkg2 pkg3 pkgN ...
```

For help with other functions, run the following command:
```
$ scratch help
```

## pkgbuild

`pkgbuild` is a tool to build a package from a package's recipe (called port). 
For normal users, `scratch` will take care of building and install packages.

## pkgadd

`pkgadd` is a tool to install precompiled packages. The usage is:
```
# pkgadd <pkg>.spkg.tar.xz
```

## pkgdel

`pkgdel` is a tool to remove installed packages from your system. The usage is:
```
# pkgdel <pkg>
```

## revdep

`revdep` is a tool to find and fix (rebuild and reinstall pkgs) broken or missing dependencies.
This script is recommended to be run after **upgrading** and **removing** packages.

## updateconf

`updateconf` is a tool to update configuration files in `/etc`. This script is recommended
to be run after **upgrading** packages.

> NOTE: For all tools aside from `scratch`, run `<tool> -h` or `<tool> --help` to get usage info.
