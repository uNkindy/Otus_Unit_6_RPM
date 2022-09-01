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
* Достал из архива SPEC-файл __httpd.spec___. Скопировал архив в папку ~/SOURCES/, SPEC-файл в ~/SPECS/.
* Создал src rpm пакет.
```console
[root@server rpmbuild]# rpmbuild -bs SPECS/httpd.spec
Wrote: /root/rpmbuild/SRPMS/httpd-2.4.54-1.src.rpm
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
* 

