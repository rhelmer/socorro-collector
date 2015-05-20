Socorro Collector
-----------------

A WSGI server for collecting multi-part HTTP form posts, and storing to
a number of different backends (S3, PostgreSQL, ElasticSearch, HBase, RabbitMQ,
filesystem - or any combination of these)

Collector requires the ProductName and Version fields to be set. For example,
to start the Collector using a Procfile runner (foreman or honcho should work)
and submit a crash:

export PORT=8888
honcho start web
curl -F 'ProductName=Blah' -F 'Version=1.0' 'http://localhost:8888/submit'

You can specify any other form fields you like. A "CrashID" should be returned,
and you can find your data in the `./crashes/` directory.

This project is used by the Socorro crash collecting project for collecting
crash reports from clients using the [Breakpad libraries](http://code.google.com/p/google-breakpad/), but can be used to retrieve any payload.

Documentation:
http://socorro.readthedocs.org

Source code:
https://github.com/mozilla/socorro
https://github.com/rhelmer/socorro-collector

Releases:
https://github.com/mozilla/socorro/releases

Installation instructions:
http://socorro.readthedocs.org/en/latest

Socorro mailing list:
https://lists.mozilla.org/listinfo/tools-socorro

Socorro/Breakpad IRC channel:
irc://irc.mozilla.org/breakpad
