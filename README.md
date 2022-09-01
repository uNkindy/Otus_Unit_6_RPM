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
* Скачал исходники [Apache HTTPD Server 2.4.54] (https://httpd.apache.org/download.cgi#apache24)
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
