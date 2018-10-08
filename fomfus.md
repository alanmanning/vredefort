# Linux

## Ensure default python version is python3 in Debian 9
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


## Configure virtualenvwrapper
(https://virtualenvwrapper.readthedocs.io/en/latest/)

```bash
install virtualenvwrapper
pip install virtualenvwrapper
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
lssitpackages - see what packages are installed in the virtualenv
workon XXX - switch to XXX env



# Django

