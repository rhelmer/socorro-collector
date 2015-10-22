Socorro Collector
-----------------

Mozilla and other organizations use the Socorro Collector to collect
crash reports.

Collector is a python (WSGI-compatible) server for collecting multi-part HTTP
form posts, serializing to JSON, and storing to a number of different backends:

- S3, PostgreSQL, ElasticSearch, HBase, filesystem - or any
combination of these.

RabbitMQ can be used to queue incoming crashes for processing.  
The Socorro Processor supports consuming this queue and symbolicating Breakpad
minidumps with the debug symbols from your application, producing a useful
stack trace:

https://github.com/rhelmer/socorro-processor


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
  export web_server__port='8888'
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

You can set these in the environment or put then in a `.env` file.

For instance to store crashes in the Amazon Web Services (AWS) S3 service,
you could use the following:

```
  # Store the crash in S3
  storage__crashstorage_class='socorro.external.boto.crashstorage.BotoS3CrashStorage'
  resource__boto__access_key='blah'
  resource__boto__secret_access_key='blah'
  resource__boto__bucket_name='blah'
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

Other Socorro Crash Reporting apps:
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
