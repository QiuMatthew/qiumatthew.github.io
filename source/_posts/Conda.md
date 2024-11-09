---
title: Conda
auther: yuran
date: 2021-10-02 13:54:41
categories:
    - 技术
    - Conda
tags:
    - 技术
    - Conda
---

## Anaconda & Miniconda
Anaconda is conda with some frequently used(although I disagree) preinstalled packages
Miniconda is conda with only base modules, and we have to install every dependencies we need

I prefer Miniconda

<!--more-->

## Installing
### Download
```
>>> wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```
### Install
```
>>> ./Miniconda3-latest-Linux-x86_64.sh
```
初始化可以选yes（会修改环境变量）
```
Do you wish the installer to initialize Miniconda3
by running conda init? [yes|no]
[no] >>> yes
```
重启后即可


## Using
### Create Environment
```
>>> conda create --name env-for-torch python=3.9
```
```
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /home/matthew/miniconda3/envs/env-for-torch

  added / updated specs:
    - python=3.9


The following NEW packages will be INSTALLED:

  _libgcc_mutex      pkgs/main/linux-64::_libgcc_mutex-0.1-main
  _openmp_mutex      pkgs/main/linux-64::_openmp_mutex-4.5-1_gnu
  ca-certificates    pkgs/main/linux-64::ca-certificates-2021.7.5-h06a4308_1
  certifi            pkgs/main/linux-64::certifi-2021.5.30-py39h06a4308_0
  ld_impl_linux-64   pkgs/main/linux-64::ld_impl_linux-64-2.35.1-h7274673_9
  libffi             pkgs/main/linux-64::libffi-3.3-he6710b0_2
  libgcc-ng          pkgs/main/linux-64::libgcc-ng-9.3.0-h5101ec6_17
  libgomp            pkgs/main/linux-64::libgomp-9.3.0-h5101ec6_17
  libstdcxx-ng       pkgs/main/linux-64::libstdcxx-ng-9.3.0-hd4cf53a_17
  ncurses            pkgs/main/linux-64::ncurses-6.2-he6710b0_1
  openssl            pkgs/main/linux-64::openssl-1.1.1l-h7f8727e_0
  pip                pkgs/main/linux-64::pip-21.2.4-py37h06a4308_0
  python             pkgs/main/linux-64::python-3.9.7-h12debd9_1
  readline           pkgs/main/linux-64::readline-8.1-h27cfd23_0
  setuptools         pkgs/main/linux-64::setuptools-58.0.4-py39h06a4308_0
  sqlite             pkgs/main/linux-64::sqlite-3.36.0-hc218d9a_0
  tk                 pkgs/main/linux-64::tk-8.6.11-h1ccaba5_0
  tzdata             pkgs/main/noarch::tzdata-2021a-h5d7bf9c_0
  wheel              pkgs/main/noarch::wheel-0.37.0-pyhd3eb1b0_1
  xz                 pkgs/main/linux-64::xz-5.2.5-h7b6447c_0
  zlib               pkgs/main/linux-64::zlib-1.2.11-h7b6447c_3


Proceed ([y]/n)? y

Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use
#
#     $ conda activate env-for-torch
#
# To deactivate an active environment, use
#
#     $ conda deactivate
``` 

### Activate & Deactivate
```
>>> conda activate env-for-torch
>>> conda deactivate
```

### Check existing environment
```
>>> conda env list
```
```
# conda environments:
#
base                  *  /home/matthew/miniconda3
env-for-tf               /home/matthew/miniconda3/envs/env-for-tf
env-for-torch            /home/matthew/miniconda3/envs/env-for-torch
```
