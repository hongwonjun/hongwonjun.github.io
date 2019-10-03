---
layout: post
title: "Ubuntu 에서 MySQL-python 설치시 오류 해결"
date: 2016-06-22 00:00:00 +0900
author: Hong Wonjun
categories: Trouble-Shooting
tags: python, ubuntu, mysql
---

Python 에서 SQLAlchemy 를 사용하여 MariaDB (혹은 MySQL) 을 사용할 때 아래와 같은 메세지가 발생했다.

{% highlight python %}
ImportError: No module named MySQLdb
{% endhighlight %}

MySQL-python 을 pip 로 설치 시도.

{% highlight bash %}
$ sudo pip install MySQL-python
Downloading/unpacking MySQL-python
  Downloading MySQL-python-1.2.5.zip (108kB): 108kB downloaded
  Running setup.py (path:/tmp/pip_build_root/MySQL-python/setup.py) egg_info for package MySQL-python
    sh: 1: mysql_config: not found
    Traceback (most recent call last):
      File "<string>", line 17, in <module>
      File "/tmp/pip_build_root/MySQL-python/setup.py", line 17, in <module>
        metadata, options = get_config()
      File "/tmp/pip_build_root/MySQL-python/setup_posix.py", line 43, in get_config
        libs = mysql_config("libs_r")
      File "/tmp/pip_build_root/MySQL-python/setup_posix.py", line 25, in mysql_config
        raise EnvironmentError("%s not found" % (mysql_config.path,))
    EnvironmentError: mysql_config not found
    Complete output from command python setup.py egg_info:
    sh: 1: mysql_config: not found

Traceback (most recent call last):
  File "<string>", line 17, in <module>
  File "/tmp/pip_build_root/MySQL-python/setup.py", line 17, in <module>
    metadata, options = get_config()
  File "/tmp/pip_build_root/MySQL-python/setup_posix.py", line 43, in get_config
    libs = mysql_config("libs_r")
  File "/tmp/pip_build_root/MySQL-python/setup_posix.py", line 25, in mysql_config
    raise EnvironmentError("%s not found" % (mysql_config.path,))

EnvironmentError: mysql_config not found

----------------------------------------
Cleaning up...
Command python setup.py egg_info failed with error code 1 in /tmp/pip_build_root/MySQL-python
{% endhighlight %}

python-dev 와 libmysqlclient-dev 를 설치하라고 하는데 libmysqlclient-dev 설치시 아래와 같은 에러가 발생한다.

{% highlight bash %}
$ sudo apt-get install libmysqlclient-dev
Reading package lists... Done
Building dependency tree
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 libmysqlclient-dev : Depends: libmysqlclient18 (= 5.5.49-0ubuntu0.14.04.1) but 10.0.25+maria-1~trusty is to be installed
E: Unable to correct problems, you have held broken packages.
{% endhighlight %}

돌아가서 MySQL-python 을 apt-get 으로 설치해서 문제를 해결한다.

{% highlight bash %}
$ sudo apt-get install python-MySQLdb
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libntdb1 python-ntdb
Use 'apt-get autoremove' to remove them.
Suggested packages:
  python-egenix-mxdatetime python-mysqldb-dbg
The following NEW packages will be installed:
  python-mysqldb
0 upgraded, 1 newly installed, 0 to remove and 8 not upgraded.
Need to get 55.4 kB of archives.
After this operation, 196 kB of additional disk space will be used.
Get:1 http://ftp.daumkakao.com/ubuntu/ trusty/main python-mysqldb amd64 1.2.3-2ubuntu1 [55.4 kB]
Fetched 55.4 kB in 0s (120 kB/s)
Selecting previously unselected package python-mysqldb.
(Reading database ... 206384 files and directories currently installed.)
Preparing to unpack .../python-mysqldb_1.2.3-2ubuntu1_amd64.deb ...
Unpacking python-mysqldb (1.2.3-2ubuntu1) ...
Setting up python-mysqldb (1.2.3-2ubuntu1) ...
{% endhighlight %}
