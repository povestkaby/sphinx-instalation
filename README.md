# sphinxsearch-v3-install
 
*Contains several helper-config files to make it easier to install and run SphinxSearch v3+.*
  
So below is an example to install SphinxSearch v3+ on such OS.

 **1. Get latest distribution from http://sphinxsearch.com**

 **2. Create user to run SphinxSearch**

`useradd -r -U -c 'Sphinxsearch system user' sphinx`

 **3. Unarchive repo contents in folder**
 
 You'll get something like this as directory structure:
 
 ```text
sphinx-3.x.x
├── api
│   └── ...
├── bin
│   ├── indexer
│   ├── indextool
│   ├── searchd
│   └── wordbreaker
├── doc
│   └── ...
├── etc
│   └── ...
├── misc
│   └── ...
└── src
    └── ...
```
 We are interested in ./bin folder contents only. Just copy ./bin files into your /usr/bin folder.
 
 `cp sphinx-3.x.x/bin/* /usr/bin`
 
 Test that searchd - SphinxSearch daemon binary now exists in your system
 
 `whereis searchd`
 
 You'll get 
 
> searchd: /usr/bin/searchd

**4. Configure our installation**

I have my own example config, you may take yours.

First - create paths we need to store indexes, config files, logs and etc.

`mkdir -p /etc/sphinx /etc/sphinx/data /etc/sphinx/log`

Let our created sphinx user to deal with new paths.

`chown -R sphinx:sphinx /etc/sphinx /etc/sphinx/data /etc/sphinx/log`

Move config files from this repository /etc folder to their places.

General config:
> /etc/sphinx/sphinx.conf


Systemd service file:
> /lib/systemd/system/sphinx.service

**5. Enable systemd service**
 Enter this to enable system service:
 
`systemctl enable sphinx`
 
 You'l get something like:
```test
Created symlink /etc/systemd/system/sphinx.service → /lib/systemd/system/sphinx.service.
```
 
**5. Start the service**

Now you can start SphinxSearch daemon as

`service sphinx start`

**6.Check installation**

To check the service you can use:

`ps ax | grep searchd`

```text
  3744 ?        S      0:00 /usr/bin/searchd --config /etc/sphinx/sphinx.conf
  3746 ?        Sl     0:00 /usr/bin/searchd --config /etc/sphinx/sphinx.conf
```

SphinxSearch supports connections via mysql-like interface, so to perform additional check we need:

`mysql -uroot -h 127.0.0.1 -P 9306`

Note: for this check you'll have to install mysql-client library (ex: mysql-client-core-8.0)

In opened console you can see that SphinxSearch created our test news index

```test
mysql> show tables;
+-------+------+
| Index | Type |
+-------+------+
| news  | rt   |
+-------+------+
1 row in set (0.00 sec)
```

Reboot the system/virtual machine and see the service running.

