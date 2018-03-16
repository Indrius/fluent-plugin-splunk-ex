[![Build Status](https://travis-ci.org/gtrevg/fluent-plugin-splunk-ex.svg?branch=master)](https://travis-ci.org/gtrevg/fluent-plugin-splunk-ex)  [![Code Climate](https://codeclimate.com/github/gtrevg/fluent-plugin-splunk-ex.png)](https://codeclimate.com/github/gtrevg/fluent-plugin-splunk-ex)  [![Coverage Status](https://coveralls.io/repos/gtrevg/fluent-plugin-splunk-ex/badge.png)](https://coveralls.io/r/gtrevg/fluent-plugin-splunk-ex)

## Overview

This plugin will send your fluentd logs to a splunk server.  It can send the data in either
key/value (k1=v1 k2=v2) or json format for easy splunk parsing.

Modified the original project to enable Openshift-Logentries integration (new "output_format" setting available: "logentries").
The "logentries" output produces a human-readable string containing a label (for Logentries log matching), hostname, log level and log message.


## Installation

    gem install fluent-plugin-splunk-ex

## Configuration

### Plugin

    <match pattern>
      type splunk_ex
      host <splunk_host>                 # default: localhost
      port <splunk_port>                 # default: 9997 - but you'll want to change this
      output_format json|kv|logentries   # default: json
    </match>

### Splunk

You may need to open up a special TCP port just for the fluentd logs.  To do that, go to
`Manager -> Data Inputs -> TCP -> New`.  Then decide the following:

* Port
* Source Name
* Source Type
* Index ( default works well )

After enabling these settings, you'll be able to see your fluentd logs appear in your Splunk search interface.
The JSON format will be automagically parsed and indexed based on the keys passed in.

Because the plugin batch sends data to Splunk, you'll want to update your `apps/search/local/props.conf`
file to specify that Splunk should split on newlines. If you do not update this setting, you find that
all logs from a similar time slice will be stacked upon each other.  Because the kv & json formats do
not contain any newline characters, splitting on the newline will solve this problem.  The values to
add to this file are:

    [<source_type_here>]
    SHOULD_LINEMERGE = false
    
This will make sure that the new source type you just set up for fluentd will always split on the newline character.

## Copyright

Copyright (c) 2014 Trevor Gattis

## License

Apache License, Version 2.0


