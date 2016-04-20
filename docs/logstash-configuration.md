### LogStash setup

Primary aim of the logstash set-up is to:

1. Frequently poll for new email
2. Process emails, tag them appropriately and ingest them into the given elastic search cluster.

All of the emails are supposed to be processed from the mail server's (postfix) file system location. 
This can be done by configuring [logstash-input-file](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-file.html) module.

Sample configuration for this plugin will look like:
```
file {
    path => /var/lib/email/*
}
```
Additional attributes can also be tagged (such as timestamp)

Another alternative is have logstash directly poll the imap mail server via the [imap-mail-server](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-imap.html) plugin 

Sample Configuration:
```
# /etc/logstash/conf.d/01-imap-input.conf
input {
  imap {
	host => "imap.yourmailserver.com"
	user => "username"
	password => "password"
	secure => true
	fetch_count => 15
	folder => "alerts"
  }
}
```
