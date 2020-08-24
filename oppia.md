# Oppia

To set up the development environment to contribute to [Oppia](https://github.com/oppia/oppia) on Arch, do the following.

Install dependencies:

```sh
yay -S curl jre7-openjdk unzip git chromium python-virtualenv
```

Set up a virtual environment:

```sh
virtualenv -p /usr/bin/python2.7 venv
source venv/bin/activate
pip install pyyaml
```
