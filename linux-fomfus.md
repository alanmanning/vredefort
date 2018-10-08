# Linux

## When you don't care about precise version of python

### Ensure default python version is python3 in Debian 9
(https://linuxconfig.org/how-to-change-default-python-version-on-debian-9-stretch-linux)

Check if you need to do this. From command line:
```bash
python --version
```
if it's python3 then you're done

See if there are any current alternatives:
```bash
sudo update-alternatives --list python
```
Will probably say no alternatives for python

List all available options:
```bash
ls /usr/bin/python*
```
See what versions are there. Probably will have a Python 2 and 3.

Add options:
```bash
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.1 2
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.5 1
```
The integer at the end lists the priority. So python3.5 will be run first when in auto mode

Now try
```bash
python --version
```
it's probably still python2. So need to switch:
```bash
sudo update-alternatives --config python
```
choose python3

check that it worked
```bash
python --version
```
should be python 3

### Configure virtualenvwrapper
(https://virtualenvwrapper.readthedocs.io/en/latest/)

```bash
install virtualenvwrapper
pip install --updgrade --user virtualenvwrapper
```

add to .bashrc:
```bash
export WORKON_HOME=~/Envs
source /usr/local/bin/virtualenvwrapper.sh
```

When in folder you want to do work in:
```
mkvirtualenv env1
```
All venvs's are stored in ~/Envs. Don't need to back this stuff up.

Useful commands:

 * lssitpackages - see what packages are installed in the virtualenv

 * workon XXX - switch to XXX env

# AWS CLI install

Install it for the user, not for everyone
```bash
pip install awsebcli --upgrade --user
```

It may complain that ~/.local/bin is not in PATH. If so, add to .bashrc:
```bash
export PATH="$PATH:/home/alan/.local/bin"
```


## Getting the right version of python
AWS EB [only runs](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/concepts.platforms.html#concepts.platforms.python) Python 3.6.5, Python 3.4.8, Python 2.7.14, or Python 2.6.9

We'd like the development version of Python to be the same as the one on AWS. Unfortunately, not straightforward for some OSes, because package managers only install a specific version, and installing another version system-wide isn't a great idea.

We can get around this on the AWS side by using Docker containers, which lets us run whatever version of Python we want. Alternatively, we can run a different version of Python inside a virtualenv on the development machines.

### Install a local copy of a specific python version
(http://thomas-cokelaer.info/blog/2014/08/installing-another-python-version-into-virtualenv/)

First, need to install some libraries. If you don't do this, Python3.6 might complain about not having ssh. I don't know why this works (https://stackoverflow.com/a/48725232/3537049)

```bash
sudo apt-get install build-essential checkinstall
sudo apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev
```

Now, download and build Python3.6 from source.
```bash
cd ~/alan/
mkdir software
cd software
wget  https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tar.xz
unxz Python-3.6.5.tar.xz
tar xvf Python-3.6.5.tar
cd Python-3.6.5
./configure --enable-optimizations
make altinstall
```

make altinstall prevents this version of python from integrating itself into the OS and screwing up existing Python versions.

### Configure a virtualenv to run a precise version of python

```bash
mkvirtualenv --python=/usr/local/bin/python3.6 web-devel
```
Once in the virtualenv, you can install everything you need.