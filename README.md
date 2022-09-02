### Домашнее задание №6 (RMP)
#### 1. Создан [Vagrantfile](https://github.com/uNkindy/Otus_Unit_6_RPM/blob/main/Vagrantfile)
* Установлены пакеты для сборки rpm пакетов:
```console
[root@server vagrant]# yum group install "RPM Development Tools"

...

Installed:
  binutils-2.30-113.el8.x86_64             elfutils-0.186-1.el8.x86_64         
  emacs-filesystem-1:26.1-7.el8.noarch     gc-7.6.4-3.el8.x86_64               
  gdb-headless-8.2-18.el8.x86_64           guile-5:2.0.14-7.el8.x86_64         
  libatomic_ops-7.6.2-3.el8.x86_64         libbabeltrace-1.5.4-3.el8.x86_64    
  libipt-1.6.1-8.el8.x86_64                libtool-ltdl-2.4.6-25.el8.x86_64    
  patch-2.7.6-11.el8.x86_64                rpm-build-4.14.3-23.el8.x86_64      
  rpmdevtools-8.10-8.el8.noarch            zstd-1.4.4-1.el8.x86_64             

Complete!
```
* Подготовил окружение для сборки rpm пакетов:
```console
[root@server vagrant]# rpmdev-setuptree
[root@server vagrant]# ls /root/rpmbuild/
BUILD  RPMS  SOURCES  SPECS  SRPMS
```
* Скачал исходники [Apache HTTPD Server 2.4.54](https://httpd.apache.org/download.cgi#apache24)
```console
[root@server vagrant]# wget https://dlcdn.apache.org/httpd/httpd-2.4.54.tar.bz2
--2022-09-01 17:13:19--  https://dlcdn.apache.org/httpd/httpd-2.4.54.tar.bz2
Resolving dlcdn.apache.org (dlcdn.apache.org)... 151.101.2.132
Connecting to dlcdn.apache.org (dlcdn.apache.org)|151.101.2.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 9743277 (9.3M) [application/x-gzip]
Saving to: 'httpd-2.4.54.tar.bz2'

httpd-2.4.54.tar.gz 100%[===================>]   9.29M  8.64MB/s    in 1.1s    

2022-09-01 17:13:20 (8.64 MB/s) - 'httpd-2.4.54.tar.bz2' saved [9743277/9743277]

```
* Достал из архива SPEC-файл __httpd.spec___. Скопировал архив в папку ~/SOURCES/, SPEC-файл в ~/SPECS/. Удалил модуль heartbeat и proxy-balancer из конфигурации:
```console
%configure \
	--enable-layout=RPM \
        --libdir=%{_libdir} \
        --sysconfdir=%{_sysconfdir}/httpd/conf \
        --includedir=%{_includedir}/httpd \
        --libexecdir=%{_libdir}/httpd/modules \
        --datadir=%{contentdir} \
        --with-installbuilddir=%{_libdir}/httpd/build \
        --enable-mpms-shared=all \
        --with-apr=%{_prefix} --with-apr-util=%{_prefix} \
        --enable-suexec --with-suexec \
        --with-suexec-caller=%{suexec_caller} \
        --with-suexec-docroot=%{contentdir} \
        --with-suexec-logfile=%{_localstatedir}/log/httpd/suexec.log \
        --with-suexec-bin=%{_sbindir}/suexec \
        --with-suexec-uidmin=500 --with-suexec-gidmin=100 \
        --enable-pie \
        --with-pcre \
        --enable-mods-shared=all \
        --enable-ssl --with-ssl --enable-bucketeer \
        --enable-case-filter --enable-case-filter-in \
        --disable-imagemap \
        --enable-heartbeat=no \
        --enable-proxy-balancer=no \
```
* Для сборки rpm пакета __https__ были установлены необходимые пакеты:
```console
[root@server rpmbuild]# yum install -y apr-devel apr-util-devel autoconf libselinux-devel libuuid-devel libxml2-devel lua-devel openldap-devel openssl-devel pcre-devel perl

...

Installed:
  apr-1.6.3-12.el8.x86_64               apr-devel-1.6.3-12.el8.x86_64          
  apr-util-1.6.1-6.el8.x86_64           apr-util-bdb-1.6.1-6.el8.x86_64        
  apr-util-devel-1.6.1-6.el8.x86_64     apr-util-openssl-1.6.1-6.el8.x86_64    
  autoconf-2.69-29.el8.noarch           cmake-filesystem-3.20.2-4.el8.x86_64   
  cyrus-sasl-2.1.27-6.el8_5.x86_64      cyrus-sasl-devel-2.1.27-6.el8_5.x86_64 
  expat-devel-2.2.5-8.el8_6.2.x86_64    keyutils-libs-devel-1.5.10-9.el8.x86_64
  krb5-devel-1.18.2-14.el8.x86_64       libcom_err-devel-1.45.6-4.el8.x86_64   
  libdb-devel-5.3.28-42.el8_4.x86_64    libkadm5-1.18.2-14.el8.x86_64          
  libselinux-devel-2.9-5.el8.x86_64     libsepol-devel-2.9-3.el8.x86_64        
  libuuid-devel-2.32.1-35.el8.x86_64    libverto-devel-0.3.0-5.el8.x86_64      
  libxml2-devel-2.9.7-13.el8_6.1.x86_64 m4-1.4.18-7.el8.x86_64                 
  make-1:4.2.1-11.el8.x86_64            openldap-devel-2.4.46-18.el8.x86_64    
  openssl-devel-1:1.1.1k-7.el8_6.x86_64 pcre-cpp-8.42-6.el8.x86_64             
  pcre-devel-8.42-6.el8.x86_64          pcre-utf16-8.42-6.el8.x86_64           
  pcre-utf32-8.42-6.el8.x86_64          pcre2-devel-10.32-2.el8.x86_64         
  pcre2-utf16-10.32-2.el8.x86_64        pcre2-utf32-10.32-2.el8.x86_64         
  perl-4:5.26.3-421.el8.x86_64          perl-CPAN-2.18-397.el8.noarch          
  xz-devel-5.2.4-4.el8_6.x86_64        

Complete!
```
* Собрал комплект rpm-пакетов __httpd__.
```console
Checking for unpackaged file(s): /usr/lib/rpm/check-files /root/rpmbuild/BUILDROOT/httpd-2.4.54-1.x86_64
Wrote: /root/rpmbuild/RPMS/x86_64/httpd-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/httpd-devel-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/httpd-manual-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/httpd-tools-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/mod_authnz_ldap-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/mod_lua-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/mod_proxy_html-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/mod_ssl-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/httpd-debugsource-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/httpd-debuginfo-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/httpd-devel-debuginfo-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/httpd-tools-debuginfo-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/mod_authnz_ldap-debuginfo-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/mod_lua-debuginfo-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/mod_proxy_html-debuginfo-2.4.54-1.x86_64.rpm
Wrote: /root/rpmbuild/RPMS/x86_64/mod_ssl-debuginfo-2.4.54-1.x86_64.rpm
Executing(%clean): /bin/sh -e /var/tmp/rpm-tmp.tVpdy9
+ umask 022
+ cd /root/rpmbuild/BUILD
+ cd httpd-2.4.54
+ rm -rf /root/rpmbuild/BUILDROOT/httpd-2.4.54-1.x86_64
+ exit 0
[root@server rpmbuild]# ls RPMS/x86_64/
httpd-2.4.54-1.x86_64.rpm
httpd-debuginfo-2.4.54-1.x86_64.rpm
httpd-debugsource-2.4.54-1.x86_64.rpm
httpd-devel-2.4.54-1.x86_64.rpm
httpd-devel-debuginfo-2.4.54-1.x86_64.rpm
httpd-manual-2.4.54-1.x86_64.rpm
httpd-tools-2.4.54-1.x86_64.rpm
httpd-tools-debuginfo-2.4.54-1.x86_64.rpm
mod_authnz_ldap-2.4.54-1.x86_64.rpm
mod_authnz_ldap-debuginfo-2.4.54-1.x86_64.rpm
mod_lua-2.4.54-1.x86_64.rpm
mod_lua-debuginfo-2.4.54-1.x86_64.rpm
mod_proxy_html-2.4.54-1.x86_64.rpm
mod_proxy_html-debuginfo-2.4.54-1.x86_64.rpm
mod_ssl-2.4.54-1.x86_64.rpm
mod_ssl-debuginfo-2.4.54-1.x86_64.rpm
```
* Создал командой __mkdir__ папку для локального репозитория __/home/vagrant/repos/almalinux/8__.
* Установил пакет для создания rpm репозиториев __createrepo__.
* Скопировал собранные rpm пакеты в директорию репозитория.
```console
[root@server 8]# cp /root/rpmbuild/RPMS/x86_64/*.rpm /home/vagrant/repos/almalinux/8/
```
*Создал репозиторий в директории:
```console
[root@server 8]# createrepo /home/vagrant/repos/almalinux/8/
Directory walk started
Directory walk done - 16 packages
Temporary output repo path: /home/vagrant/repos/almalinux/8/.repodata/
Preparing sqlite DBs
Pool started (with 5 workers)
Pool finished
```
* Создал конфигурационный файл локального репозитория:
```console
[root@server 8]# cat /etc/yum.repos.d/local.repo 
[local]
name=Local
baseurl=file:///home/vagrant/repos/almalinux/8/
enabled=1
gpgcheck=0
```
* Удалил текущую версию __httpd__ 2.4.37:
```console
[root@server 8]# yum remove httpd
Failed to set locale, defaulting to C.UTF-8
Dependencies resolved.
=============================================================================================================================================================================================================
 Package                                       Architecture                                   Version                                            Repository                                             Size
=============================================================================================================================================================================================================
Removing:
 httpd                                         x86_64                                         2.4.37                                           @@commandline                                         4.5 M

Transaction Summary
=============================================================================================================================================================================================================
Remove  1 Package

Freed space: 4.5 M
Is this ok [y/N]: n
Operation aborted.
[root@server 8]# yum remove httpd
Failed to set locale, defaulting to C.UTF-8
Dependencies resolved.
=============================================================================================================================================================================================================
 Package                                       Architecture                                   Version                                            Repository                                             Size
=============================================================================================================================================================================================================
Removing:
 httpd                                         x86_64                                         2.4.37                                           @@commandline                                         4.5 M

Transaction Summary
=============================================================================================================================================================================================================
Remove  1 Package

Freed space: 4.5 M
Is this ok [y/N]: y
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                                     1/1 
  Running scriptlet: httpd-2.4.37.x86_64                                                                                                                                                               1/1 
  Erasing          : httpd-2.4.37.x86_64                                                                                                                                                               1/1 
  Running scriptlet: httpd-2.4.37.x86_64                                                                                                                                                               1/1 
  Verifying        : httpd-2.4.37.x86_64                                                                                                                                                               1/1 

Removed:
  httpd-2.4.37.x86_64                                                                                                                                                                                      

Complete!
```
* Отключил все основне репозитории в almalinux.
```console
[root@server 8]# cat /etc/yum.repos.d/almalinux.repo 
# almalinux.repo

[baseos]
name=AlmaLinux $releasever - BaseOS
mirrorlist=https://mirrors.almalinux.org/mirrorlist/$releasever/baseos
# baseurl=https://repo.almalinux.org/almalinux/$releasever/BaseOS/$basearch/os/
enabled=0
gpgcheck=1
countme=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-AlmaLinux

[appstream]
name=AlmaLinux $releasever - AppStream
mirrorlist=https://mirrors.almalinux.org/mirrorlist/$releasever/appstream
# baseurl=https://repo.almalinux.org/almalinux/$releasever/AppStream/$basearch/os/
enabled=0
gpgcheck=1
countme=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-AlmaLinux

[extras]
name=AlmaLinux $releasever - Extras
mirrorlist=https://mirrors.almalinux.org/mirrorlist/$releasever/extras
# baseurl=https://repo.almalinux.org/almalinux/$releasever/extras/$basearch/os/
enabled=0
gpgcheck=1
countme=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-AlmaLinux
```
* Проверил доступность локального репозитория:
```console
[root@server 8]# yum repolist enabled
Failed to set locale, defaulting to C.UTF-8
repo id                                                                                                 repo name
local     
```
* Установил собранный rpm пакет с последней версией __httpd__ при помщи __yum__:
```console
[root@server 8]# yum install httpd-2.4.54-1.x86_64.rpm
Failed to set locale, defaulting to C.UTF-8
Last metadata expiration check: 1:53:30 ago on Fri Sep  2 13:43:43 2022.
Dependencies resolved.
=============================================================================================================================================================================================================
 Package                                       Architecture                                   Version                                             Repository                                            Size
=============================================================================================================================================================================================================
Installing:
 httpd                                         x86_64                                         2.4.54-1                                            @commandline                                         1.4 M

Transaction Summary
=============================================================================================================================================================================================================
Install  1 Package

Total size: 1.4 M
Installed size: 4.5 M
Is this ok [y/N]: y
Downloading Packages:
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                                     1/1 
  Running scriptlet: httpd-2.4.54-1.x86_64                                                                                                                                                               1/1 
  Installing       : httpd-2.4.54-1.x86_64                                                                                                                                                               1/1 
  Running scriptlet: httpd-2.4.54-1.x86_64                                                                                                                                                               1/1 
  Verifying        : httpd-2.4.54-1.x86_64                                                                                                                                                               1/1 

Installed:
  httpd-2.4.54-1.x86_64                                                                                                                                                                                      

Complete!
```
* Запустил процесс __httpd__:
```console
[root@server 8]# systemctl start httpd
[root@server 8]# systemctl status httpd
● httpd.service - LSB: start and stop Apache HTTP Server
   Loaded: loaded (/etc/rc.d/init.d/httpd; generated)
   Active: active (running) since Fri 2022-09-02 15:38:21 UTC; 4s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 232405 ExecStart=/etc/rc.d/init.d/httpd start (code=exited, status=0/SUCCESS)
 Main PID: 232418 (httpd)
    Tasks: 82 (limit: 5952)
   Memory: 13.3M
   CGroup: /system.slice/httpd.service
           ├─232418 /usr/sbin/httpd
           ├─232420 /usr/sbin/httpd
           ├─232421 /usr/sbin/httpd
           └─232422 /usr/sbin/httpd

Sep 02 15:38:21 server systemd[1]: Starting LSB: start and stop Apache HTTP Server...
Sep 02 15:38:21 server httpd[232405]: Starting httpd:
Sep 02 15:38:21 server httpd[232417]: Starting httpd:
Sep 02 15:38:21 server httpd[232417]: H00558: httpd: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppre
Sep 02 15:38:21 server systemd[1]: httpd.service: Can't open PID file /var/run/httpd.pid (yet?) after start: No such file or directory
Sep 02 15:38:21 server httpd[232405]: H00558: ht
Sep 02 15:38:21 server systemd[1]: Started LSB: start and stop Apache HTTP Server.
```

