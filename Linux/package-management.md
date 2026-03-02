# Linux Package Management

> A package - archive that contains all the files required for an app to run

A package manager:

* insures integrity of the package
* simplify the package management
* managing dependencies

## RPM family (RHEL, Centos, Fedora)

* Centos is a fork of Red Hat
* Red hat is paid
`rpm` - RedHat Package Manager
`yum`
`dnf`

### RPM 

* file extension: `.rpm`
* stores the info in: `/var/lib/rpm` dir
* does not resolve dependencies on its own (use YUM instead)

#### Commands

* Install package

```bash
rpm -ivh telnet.rpm
```

* Uninstall

```bash
rpm -e telnet.rpm
```

* Upgrade

```bash
rpm -uvh telnet.rpm
```

* Query

```bash
rpm -q telnet.rpm
```

* Verify

```bash
rpm -Vf <path to file>
```

### YUM - Yellow dog updater modified

* free and opensource
* The DB dir: `/etc/yum.repos.d`
* high level package manager
* auto dependency resolution
* allows defining custom repos (e.g.: `/etc/yum.repos.d/redhat.repo`)

#### YUM Commands

* List repos

```bash
yum repolist
```

* Check which package provides a command

```bash
yum provides scp
```

* Install

```bash
yum install httpd
```

* Uninstall

```bash
yum remove httpd
```

* Update

```bash
yum update <package name>
```

```bash
yum update
```

## .DEB family (Ubuntu, Debian, Linux Mint)

* `dpkg`
* `apt`
* `apt-get`

### DPKG - Debian package manager

* low level package manager
* does not do auto dependency management

#### DPKG Commands

* Install

```bash
dpkg -i telnet.deb
```

* Uninstall

```bash
dpkg -r telnet.deb
```

* List

```bash
dpkg -l telnet
```

* Status

```bash
dpkg -s telnet
```

* Verify

```bash
dpkg -p path to file
```

### APT

* frontend for dpkg
* uses repositories
* database at `etc/apt/sources.list`

#### APT Commands

* Refresh repos

```bash
apt update
```

* Upgrade available packages

```bash
apt upgrade
```

* Update repositories

```bash
apt edit-sources
```

* Install package

```bash
apt install telnet
```

* Remove package

```bash
apt remove telnet
```

* Look for a package

```bash
apt search telnet
```

* List available package

```bash
apt list
```

### APT vs APT-GET

* apt is better and more user-friendly
* search is better is apt
