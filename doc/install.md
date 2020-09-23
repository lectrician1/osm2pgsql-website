---
layout: doc
title: Installation
---

Osm2pgsql works on Linux, Windows, and macOS. We recommend you always use the
latest released versions, currently version {{ site.osm2pgsql.latest_version }}.

## <img src="{% link img/linux.png %}" width="20" height="20"/> Installing on Linux

There are [packages of osm2pgsql](https://repology.org/project/osm2pgsql)
available for many different Linux distributions. Some of them are quite old,
though, and have known bugs. If the current release is not available for your
distribution, consider installing the current release from source.

### Installing on Debian/Ubuntu

For Debian/Ubuntu installation is usually as simple as

```
apt install osm2pgsql
```

If you are using Debian Stable, we recommend using the packages from
[backports](https://backports.debian.org/). Debian maintainers are really
good at keeping these up-to-date.

### Installing on Fedora

Fedora has packages of osm2pgsql available. Install with

```
dnf install osm2pgsql
```

### Installing from Source / Git

If you can't use the packaged version of osm2pgsql for your distribution, you
can always install from source.

* To install the latest release, download the
  [latest release](https://github.com/openstreetmap/osm2pgsql/releases/latest)
  as `tar.gz` or `zip` file and unpack it.
* To install a version from git, get it with `git clone
  https://github.com/openstreetmap/osm2pgsql`.

See the *Building* section in the
[`README.md`](https://github.com/openstreetmap/osm2pgsql/blob/master/README.md)
for a list of dependencies and build instructions.

## <img src="{% link img/windows.png %}" width="20" height="20"/> Installing on Windows

### Using prebuild binaries

You can [download prebuild
binaries](https://lonvia.dev.openstreetmap.org/osm2pgsql-winbuild/releases/).
Unpack the ZIP file and you can immediately use osm2pgsql.

### Compiling from source

For native Windows compilation you need a C++11-compatible compiler. Visual
Studio 2017 & 2015 are known to work. Follow the usual steps for compiling
CMake projects.


## <img src="{% link img/apple.png %}" width="20" height="20"/> Installing on macOS

Osm2pgsql is [available on
Homebrew](https://formulae.brew.sh/formula/osm2pgsql). Install with

```
brew install osm2pgsql
```
