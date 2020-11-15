[![MIT License][license-image]][license-url]

[![Support](https://www.totaljs.com/img/button-support.png?v=2)](https://www.totaljs.com/support/)

# Installation

- __SuperAdmin__ (v9.0.0) needs latest Total.js `v4` from NPM `$ npm install total4`
- __License__: [MIT](license.txt)
- [__Documentation__](https://docs.totaljs.com/superadmin/)

__IMPORTANT__
- SuperAdmin is running on port 9999 which can be changed in `run.sh` script in `/www/superadmin/run.sh`
- SuperAdmin is auto-generating port numbers for new applications starting from `8000`. So for 100 apps you need to make sure ports `8000-8099` are free.
- SuperAdmin __must be run__ as `root`

__Install requirements:__
- __Ubuntu Server +v14__
- `curl`
- `openssl`

__SuperAdmin requirements:__
- `awk`
- `bash`
- `cat`
- `cp`
- `df`
- `du`
- `free`
- `ftp`
- `grep`
- `ifconfig`
- `last`
- `lsof`
- `mkdir`
- `netstat`
- `npm`
- `ps`
- `tail`
- `tar`
- `unzip`
- `uptime`
- `wc`
- `zip`

__To install SuperAdmin run commands bellow:__

```bash
# UBUNTU
$ sudo wget https://cdn.totaljs.com/superadmin.sh && bash superadmin.sh
```

```bash
# CENTOS
$ sudo wget https://cdn.totaljs.com/superadmin-centos.sh && bash superadmin-centos.sh
```

```bash
# ALPINE
$ sudo wget https://cdn.totaljs.com/superadmin-alpine.sh && bash superadmin-alpine.sh
```

- login __user:__ `admin`, __password:__ `admin` (credentials are stored in `/www/superadmin/config`)
- manually run (if you didn't register cron in the installation) `$ cd /www/superadmin/` and `$ bash run.sh`

__To install and run SuperAdmin with Docker:__

```bash
# CLONE this repository to the current folder
git clone https://github.com/totaljs/superadmin.git .

# BUILD with tag superadmin
docker build -t superadmin .

# RUN tag superadmin (you can also map any of the exposed ports)
docker run -p 8080:80 -it superadmin
```

---

## Where does SuperAdmin store data?

All data are stored in `/superadmin/databases/` directory. Applications are stored in `application.json`.

__IMPORTANT__: NGINX access logs are stored as `/www/logs/[appname]--access.log`

---

## How to upload my application?

__SuperAdmin__ uses a Total.js package mechanism. The mechanism creates a package file with a complete directory structure and all files. __IMPORTANT__ the latest version supports uploading of `.zip` files.

- first you have to install Total.js framework as a global module `$ npm install -g total.js`
- then perform a command below:

```bash
$ cd myapplication
$ tpm create myapp.package
```

- `tpm` creates a package file
- and this file you can upload via SuperAdmin

![Upload a new application](https://www.totaljs.com/img/superadmin-upload.png)

---

## How to upgrade my older SuperAdmin version?

Don't worry, it's very easy.

- back up file `/databases/applications.json`
- back up your credentials in `/config` file (only credentials, nothing more)
- copy all directories and files from a new version of SuperAdmin to your server
- restore your backup file `/databases/applications.json`
- restore your credentials in `/config`
- install `ftp` helper via `$ apt-get install -y ftp`
- you have to update `SSL generator` to latest version via `bash /www/superadmin/ssl.sh --update`
- restart SuperAdmin `bash run.sh`
- __clear cache__ in your web browser

__IMPORTANT__: if you have lower version than `v7.0.0` then replace `/etc/nginx/nginx.conf` for `/superadmin/nginx.conf`.

## Nice to know

Bash script `superadmin/ssl.sh` can create or renew certificate manually:

```bash
# CREATE:
$ bash ssl.sh superadmin.mydomain.com

# RENEW:
$ bash ssl.sh superadmin.mydomain.com --renew
```

Bash script `superadmin/reconfigure.sh` can reconfigure SuperAdmin from/to HTTPS:

```bash
# SET TO HTTPS AND GENERATE SSL:
$ bash reconfigure.sh y superadmin.mydomain.com y

# SET TO HTTPS WITHOUT GENERATING SSL:
$ bash reconfigure.sh y superadmin.mydomain.com

# SET TO HTTP:
$ bash reconfigure.sh n superadmin.mydomain.com
```

## Uninstall SuperAdmin

- stop all apps in SuperAdmin
- kill SuperAdmin using this command `$ kill -9 $(lsof -i :9999 | grep "total" | awk {'print $2'}) > /dev/null`
- to replace nginx.conf with backed up file use this command `$ cp /etc/nginx/nginx.conf.backup /etc/`nginx/nginx.conf
- reload nginx service nginx reload and/or just stop it `$ service nginx stop`
- if you added a cron job to start apps at system startup then remove this line `@reboot /bin/bash /www/superadmin/run.sh` from crontab using `$ crontab -e` command
- remove `/www/` folder, this will remove all ssl certificates, nginx conf files, all applications, etc.
- to remove node.js run `$ apt-get remove -y nodejs`
- to remove nginx run `$ apt-get remove -y nginx`
- to remove graphicsmagick run `$ apt-get remove -y graphicsmagick`

## Contributors

- Peter Širka (author) <petersirka@gmail.com>
- Martin Smola (support) <smola.martin@gmail.com>
- Pedro Costa (contributor) <pedro@pmcdigital.pt>
- Jonathan Dumont (contributor) <totaljs@jodumont.com>

[license-image]: https://img.shields.io/badge/license-MIT-blue.svg?style=flat
[license-url]: license.txt
