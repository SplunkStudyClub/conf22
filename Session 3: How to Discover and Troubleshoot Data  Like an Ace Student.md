# Session 3: How to Discover and Troubleshoot Data Like an Ace Student

# Search for your onboarded data
index=study_club source="/var/log/palo_endpoint.log"

# Check the tailreader for issues reported
index=_internal TAILREADER file="'/var/log/palo_endpoint.log'"

# connect to the command line
use the SSL instructions in the Study Club email

# change to splunk user
sudo su - splunk

# check for issues on Splunk's inputstatus
/opt/splunk/bin/splunk list inputstatus

# check for issues via REST
```
/opt/splunk/bin/splunk _internal/opt/splunk/bin/splunk _internal call /services/admin/inputstatus/TailingProcessor:FileStatus
 call /services/admin/inputstatus/TailingProcessor:FileStatus
 ```
 
# try to read the log file
```
cat /var/log/palo_endpoint.log
 ```
# exit from splunk user
```
exit
```
# allow access to Splunk user account
```
sudo setfacl -m u:splunk:rw /var/log/palo_endpoint.log
```
# change to splunk user
```
sudo su - splunk
```

# list file permissions
```
ll /var/log
```

# open log file in VI
```
vi  /var/log/palo_endpoint.log
:q! to (quit VI)
```

# Check status of fishbucket
```
/opt/splunk/bin/splunk cmd btprobe -d /opt/splunk/var/lib/splunk/fishbucket/splunk_private_db/ --file /var/log/paloconfig.log
```
# restart Splunk
```
/opt/splunk/bin/splunk restart
```

# Search for your onboarded data
index=study_club


# Problem 2 sourcetype was set to palo_alto_logs rather than pan:config
# DISCOVERY

# check location of inputs.conf
```
/opt/splunk/bin/splunk btool inputs list --debug | grep palo
```

# check location of props.conf
```
/opt/splunk/bin/splunk btool props list --debug | grep palo
```

# Check status of fishbucket
```
/opt/splunk/bin/splunk cmd btprobe -d /opt/splunk/var/lib/splunk/fishbucket/splunk_private_db/ --file /var/log/paloconfig.log
```

# Stop Splunk
```
/opt/splunk/bin/splunk stop
```

# Clear out fishbucket 
```
/opt/splunk/bin/splunk cmd btprobe -d /opt/splunk/var/lib/splunk/fishbucket/splunk_private_db/ --file /var/log/paloconfig.log --reset
```
# Check status of fishbucket
```
/opt/splunk/bin/splunk cmd btprobe -d /opt/splunk/var/lib/splunk/fishbucket/splunk_private_db/ --file /var/log/paloconfig.log
```
# Don't Start Splunk Yet

# display props.conf
```
cat /opt/splunk/etc/apps/studyClub_all_props/default/props.conf
```
# Create a new sourcetype in correct folder
```
cd /opt/splunk/etc/apps/studyClub_all_props
ll
mkdir local
cd default
ll
cp props.conf ../local/props.conf
cd ../local
ll
```

# open props.conf in VI 
```
vi props.conf
i
```
# Props Settings - Update sourcetype palo_alto_logs to pan:config
```
[pan:config]
SHOULD_LINEMERGE=false
LINE_BREAKER=([\r\n]+)\w+\s\d+\s\d+\s\d+:\d+:\d+\s\w{4}\s
NO_BINARY_CHECK=true
CHARSET=UTF-8
MAX_TIMESTAMP_LOOKAHEAD=39
disabled=false
TIME_FORMAT=%b %d %Y %H:%M:%S
TIME_PREFIX=^
TRUNCATE=99999

[pan:endpoint]
SHOULD_LINEMERGE=false
LINE_BREAKER=([\r\n]+)\w+\s\d+\s\d+\s\d+:\d+:\d+\s\w{4}\s
NO_BINARY_CHECK=true
CHARSET=UTF-8
MAX_TIMESTAMP_LOOKAHEAD=39
disabled=false
TIME_FORMAT=%b %d %Y %H:%M:%S
TIME_PREFIX=^
TRUNCATE=99999
```
# Exit and Save
:x


# Update Inputs.conf - the sourcetype for paloconfig.log from palo_alto_logs to pan:config
```
cd /opt/splunk/etc/apps/studyClub_all_inputs/local
ll
vi inputs.conf

[monitor:///var/log/paloconfig.log]
index = study_club
sourcetype = pan:config
disabled = false

[monitor:///var/log/palo_endpoint.log]
index = study_club
sourcetype = pan:endpoint
disabled = false
```
# Exit and Save
:x

# Start Splunk (ensure you are splunk user)
```
/opt/splunk/bin/splunk start
```
# Check status of fishbucket
```
/opt/splunk/bin/splunk cmd btprobe -d /opt/splunk/var/lib/splunk/fishbucket/splunk_private_db/ --file /var/log/paloconfig.log
```
# Search for log data
index=study_club source="/var/log/paloconfig.log"

# Note that log in duplicated

# need to add the can_delete role to the splunk UI admin account 
```
index=study_club source="/var/log/paloconfig.log" sourcetype=palo_alto_logs | delete
index=study_club source="/var/log/paloconfig.log"
```

# need to remove the can_delete role to the splunk UI admin account 

# useful commands
```
head -c256 /var/log/paloconfig.log
tail -n 3 /opt/splunk/var/log/splunk/splunkd.log
```

# see what user splunk is running under
```
ps -ef | grep splunk
kill -9 <PID> 
```
