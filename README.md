# Appium Python Client Moneky

Hot Patch For [Appium-Python-Client](https://github.com/appium/python-client)

[![PyPI version](https://badge.fury.io/py/Appium-Python-Client-Monkey.svg)](https://badge.fury.io/py/Appium-Python-Client-Monkey)
[![Downloads](https://pepy.tech/badge/appium-python-client-monkey)](https://pepy.tech/project/appium-python-client-monkey)


An extension library for adding [WebDriver Protocol](https://www.w3.org/TR/webdriver/) and Appium commands to the Selenium Python language binding for use with the mobile testing framework [Appium](https://appium.io).

## Notice

Since Appium_Python-Client **v1.0.0**, only Python 3.7+ is supported.

Since Appium_Python-Client **v2.0.0**, the base selenium client version is v4.
The version only works in W3C WebDriver protocol format.
If you would like to use the old protocol (MJSONWP), please use v1 Appium Python client.
[More Reference](https://github.com/appium/python-client)

## Getting the Appium Python client Monkey

There are three ways to install and use the Appium Python client.

1. Install from [PyPi](https://pypi.org), as
['Appium-Python-Client-Monkey'](https://pypi.org/project/Appium-Python-Client-Monkey/).

    ```shell
    pip install Appium-Python-Client-Monkey
    ```

    You can see the history from [here](https://pypi.org/project/Appium-Python-Client-Mokey/#history)

2. Install from source, via [PyPi](https://pypi.org). From ['Appium-Python-Client-Monkey'](https://pypi.org/project/Appium-Python-Client-Monkey/),
download and unarchive the source tarball (Appium-Python-Client-Monkey-X.X.tar.gz).

    ```shell
    tar -xvf  Appium-Python-Client-Monkey-X.X.tar.gz
    cd Appium-Python-Client-Monkey-X.X
    python setup.py install
    ```

3. Install from source via [GitHub](https://github.com/CN-Robert-LIU/appium-python-client-monkey).

    ```shell
    git clone git@github.com:CN-Robert-LIU/appium-python-client-monkey.git
    cd appium-python-client-monkey
    python setup.py install
    ```

## Usage

The Appium Python Client is fully compliant with the WebDriver Protocol
including several helpers to make mobile testing in Python easier.

To use the new functionality now, and to use the superset of functions, instead of
including the Selenium `webdriver` module in your test code, use that from
Appium instead.

```python
from appium import webdriver

from appium_patch import monkey
webdriver.Remote = monkey.patch_all()
```

From there much of your test code will work with no change.

As a base for the following code examples, the following sets up the [UnitTest](https://docs.python.org/3/library/unittest.html)
environment:

```python
# Android environment
import unittest
from appium import webdriver
from appium_patch import monkey
webdriver.Remote = monkey.patch_all()


desired_caps = dict(
    platformName='Android',
    platformVersion='10',
    automationName='uiautomator2',
    deviceName='Android Emulator',
    app=PATH('../../../apps/selendroid-test-app.apk')
)
self.driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)
el = self.driver.find_element_by_accessibility_id('item')
el.click()
```

```python
# iOS environment
import unittest
from appium import webdriver
from appium_patch import monkey
webdriver.Remote = monkey.patch_all()


desired_caps = dict(
    platformName='iOS',
    platformVersion='13.4',
    automationName='xcuitest',
    deviceName='iPhone Simulator',
    app=PATH('../../apps/UICatalog.app.zip')
)

self.driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)
el = self.driver.find_element_by_accessibility_id('item')
el.click()
```

## Direct Connect URLs

If your Selenium/Appium server decorates the new session capabilities response with the following keys:

- `directConnectProtocol`
- `directConnectHost`
- `directConnectPort`
- `directConnectPath`

Then python client will switch its endpoint to the one specified by the values of those keys.

```python
import unittest
from appium import webdriver
from appium_patch import monkey
webdriver.Remote = monkey.patch_all()


desired_caps = dict(
    platformName='iOS',
    platformVersion='13.4',
    automationName='xcuitest',
    deviceName='iPhone Simulator',
    app=PATH('../../apps/UICatalog.app.zip')
)

self.driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps, direct_connection=True)
```

## Relax SSL validation

`strict_ssl` option allows you to send commands to an invalid certificate host like a self-signed one.

```python
import unittest
from appium import webdriver
from appium_patch import monkey
webdriver.Remote = monkey.patch_all()


desired_caps = dict(
    platformName='iOS',
    platformVersion='13.4',
    automationName='xcuitest',
    deviceName='iPhone Simulator',
    app=PATH('../../apps/UICatalog.app.zip')
)

self.driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps, strict_ssl=False)
```


## License

Apache License v2