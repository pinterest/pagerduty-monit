## pagerduty-monit

The scripts in this repository are wrappers for the PagerDuty API which will
open and close PagerDuty incidents from the command line. These scripts are
designed to help you integrate monit with PagerDuty.

Copyright (c) 2012 Cold Brew Labs, Inc. See LICENSE.md for details.

**Usage:** `pagerduty-trigger <service-name>`

**Example:** `pagerduty-trigger nginx` on the server "webserver1.example.com"
will trigger a PagerDuty incident with the subject "nginx failed on
webserver1".

**Prerequisites:** Requires pagerduty library for Python. You can download this
from http://pypi.python.org/pypi/pagerduty/0.2.1, or run `pip install
pagerduty` to download and install automatically.

**To configure PagerDuty:** To configure PagerDuty, login as an administrator
and go to the Services tab; then click "Add New Service". You can give the
service any name you'd like, and choose "Generic API system." PagerDuty will
give you an API service key for this service. Be sure to set the 
`PAGERDUTY_SERVICE_KEY` variable in both scripts.

**To configure Monit:** To configure Monit to trigger an event with this
script, save the script in a location like /etc/monit; make sure it's executable;
and use an action like this in your monitrc file:

    exec "/etc/monit/pagerduty-trigger <service-name>"

Example monit stanza to trigger an incident if nginx is not present:

    check process nginx with pidfile /var/run/nginx.pid
        if does not exist for 3 cycles
            then exec "/etc/monit/pagerduty-trigger nginx"
        else if passed for 3 cycles
            then exec "/etc/monit/pagerduty-resolve nginx"
