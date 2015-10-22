Socorro Collector
-----------------

A WSGI server for collecting multi-part HTTP form posts, and storing to
a number of different backends (S3, PostgreSQL, ElasticSearch, HBase, RabbitMQ,
filesystem - or any combination of these)

Installing dependencies
=======================
Using virtualenvwrapper and pip:
```
  mkvirtualenv collector
  pip install -r requirements.txt
```

Running
=======

Collector requires the ProductName and Version fields to be set. For example,
to start the Collector using a Procfile runner (Foreman or Honcho should work,
so should Heroku) and submit a crash.

Make sure the virtualenv is activated, and use Honcho (which was installed
above):
```
  workon collector
  export PORT=8888
  honcho start web
```

Alternatively, you can run Collector using a standalone built-in webserver
(CherryPy):
```
  workon collector
  socorro collector
```

=============
Configuration
=============

Collector supports a number of configuration formats.

Environment variables are recommended.

To see a list of all keys:

```
  socorro collector --admin.print_conf=env
```

To generate an INI-style configuration file (with commented-out documentation):
```
  socorro collector --admin.print_conf=ini
```

If you want to read a configuration file instead of using environment variables:
```
  socorro collector --admin.conf=ini
```

==================
Submitting Crashes
==================

The minimum permissible crash report contains the ProductName and Version
fields:

```
  curl -F 'ProductName=Blah' -F 'Version=1.0' 'http://localhost:8888/submit'
```

You can specify any other form fields you like. A "CrashID" should be returned,
and you can find your data in the `./crashes/` directory.

============
Data storage
============

This project is used by the Socorro crash collecting project for collecting
crash reports from clients using the [Breakpad libraries](http://code.google.com/p/google-breakpad/), but can be used to collect arbitrary data. Form fields are converted to JSON and tend to be stored this way.

=====
Tests
=====

Unit tests

```
  nosetests tests/
```

=====
Links
=====

The Socorro family of projects:
https://github.com/rhelmer/socorro-collector
https://github.com/rhelmer/socorro-processor
https://github.com/rhelmer/socorro-webapp
https://github.com/mozilla/socorro-lib

The original Socorro project:
https://github.com/mozilla/socorro

Releases:
https://github.com/mozilla/socorro/releases

Installation instructions and additional documentation:
http://socorro.readthedocs.org/en/latest

Socorro mailing list:
https://lists.mozilla.org/listinfo/tools-socorro

Socorro/Breakpad IRC channel:
irc://irc.mozilla.org/breakpad
