Log Rotate
==========

Env
---

One UWSGi process, no special log config (default log). With logrotate to rotate log files. Logrotate config:

	service.log {
	    daily
	    missingok
	    rotate 7
	    compress
	    delaycompress
	    notifempty
	    dateext
	    dateformat .%Y%m%d
	    create 0644 user user
	    sharedscripts
	    postrotate
	        [ ! -f service.pid ] || kill -HUP `/bin/cat service.pid`
	        /usr/bin/find /logs/ -name '*.gz' -type f -mtime +30 -exec /bin/rm -f {} \;
	    endscript
	}


Problem
-------

After rotate, log write to rotated log file. For example, today is 2017-02-20. Logrotate should rotate `service.log` to `service.log.20170220`. Then create a new log file `service.log`. Finally it will execute `kill -HUP`. This should make service reload and write log to `service.log`. But it keep writes to `service.log.20170220`.


Solution
--------

There are two major mode of logrotate: *copy* and *create*. ([ref][2])

* Copy mode will copy entire log content to new file. Then might truncate the original log file. Process don't need to do anything since the log file keeps exists. But the performance is poor.
* Create mode will rename the old log file. Then create a new log file. Need tell process to update log file's pointer. Usually by execute `kill -HUP`.

The problems is, the config use create mode. And execute `kill -HUP`. Which should let uWSGI write to new log file. But it didn't. To fix it. Just add `log-reopen = true` to [uWSGI config][1].

[1]:http://uwsgi-docs.readthedocs.io/en/latest/Options.html#log-reopen
[2]:http://www.linuxcommand.org/man_pages/logrotate8.html
