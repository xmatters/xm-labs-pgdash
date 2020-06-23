
# pgDash

pgDash is a comprehensive diagnostic and monitoring solution designed to help
you ensure the ongoing health and performance of your PostgreSQL deployment.
For more details, see https://pgdash.io.

pgDash supports raising xMatters events when the alerting rules you've set
have been tripped (the *Alerts* page) or when changes to your PostgreSQL server
configuration or schema have been detected (the *Change Alerts* page).

The following sections describe how to setup xMatters and pgDash so that these
events can be raised. Note that this is a one-way integration, from pgDash
to xMatters.

--------

<kbd>
<a href="https://support.xmatters.com/hc/en-us/community/topics">
   <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png">
</a>
</kbd>

--------

# Prerequisites

* pgDash Account (for [the SaaS version](https://app.pgdash.io/signup)), or
* [self-hosted pgDash](https://docs.pgdash.io/self-hosted) v2.1.0 or later
* xMatters account - If you don't have one, [get one](https://www.xmatters.com)!

# Files
* [pgDash.zip](pgDash.zip) - The xMatters *Workflow* that receives
  pgDash alerts

# How it works

The xMatters workflow provides two inbound integrations (unique
URL endpoints). pgDash does an HTTP POST to these endpoints with a payload
containing the properties as defined in the xMatters forms whenever alerts
or change alerts happen.

# Installation

## xMatters set up

1. Download the workflow [pgDash.zip](pgDash.zip) file and import 
   it into your xMatters account. See the
   [help docs](http://help.xmatters.com/OnDemand/xmodwelcome/communicationplanbuilder/exportcommplan.htm)
   for more info. You should see a new entry "pgDash"in  your
   "Workflows" page).
2. This workflow comes with two inbound integrations, one for use
   with pgDash "Alerts" and for use with pgDash "Change Alerts". Copy the
   integration URL for each of these. The "URL Authentication" type is the
   easiest to integrate with pgDash. See the [help docs](https://help.xmatters.com/ondemand/xmodwelcome/integrationbuilder/generate-urls.htm)
   for more info.

## pgDash set up

1. Once you have the 2 integration URLs, you can proceed to add them to pgDash.
   In pgDash, open the PostgreSQL server you want to monitor and navigate to the
   "Alerts" section on the sidebar. At the end of the page you can see the
   "Notifications" section (image below). Paste in the integration URL of the
   "pgdash-inbound-alert" here and click "Update".
2. Similarly, navigate to "Change Alerts" and paste in the integration URL of
   the "pgdash-inbound-change-alert" into the "Notifications" section and
   click "Update".

<kbd>
  <img src="media/pgdash-xmatters.png" width="713" height="457">
</kbd>

# Testing

To test your setup:

1. Add an alerting rule that will surely fire, for example: "Number of backends
   is greater than 1". Once the Alerts page shows that the rule has been
   triggered, check to see if xMatters events have been raised.
2. Enable Change Alerts for "List of tables" and create a table in a monitored
   database. Also set email notification. Once you get the email notification,
   check if the xMatters events have been raised.

# Troubleshooting

1. Ensure that you're using the right integration URL on the right page. The
   alert integration URL should go into the Alerts page and the Change Alert
   integration URL should go into the Change Alerts page.
2. If you're using the self-hosted version of pgDash, check the logs at
   `/var/log/pgdash/pgdash.log`.
3. For help with the pgDash side of the integration, contact support@rapidloop.com.

# Tips

1. You can also use the "Basic Authentication" and "API Key" authentication
   methods. Use URLs with embedded username and password ("https://user:pass@example.xmatters.com/")
   in these cases to supply the username+password (or API Key+secret).
