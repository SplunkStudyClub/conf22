Session 3: How to Discover and Troubleshoot Data Like an Ace Student

index=study_club source="/var/log/palo_endpoint.log"
index=_internal TAILREADER file="'/var/log/palo_endpoint.log'"
sudo su - splunk
/opt/splunk/bin/splunk list inputstatus
/opt/splunk/bin/splunk _internal/opt/splunk/bin/splunk _internal call /services/admin/inputstatus/TailingProcessor:FileStatus
 call /services/admin/inputstatus/TailingProcessor:FileStatus
 cat /var/log/palo_endpoint.log
 exit
sudo setfacl -m u:splunk:rw /var/log/palo_endpoint.log
sudo su - splunk
ll /var/log
sudo su - splunk 
vi  /var/log/palo_endpoint.log
:q! to (quit VI)
Check status of fishbucket
/opt/splunk/bin/splunk cmd btprobe -d /opt/splunk/var/lib/splunk/fishbucket/splunk_private_db/ --file /var/log/paloconfig.log
/opt/splunk/bin/splunk restart
index=study_club

Problem 2
/opt/splunk/bin/splunk btool inputs list --debug | grep palo
/opt/splunk/bin/splunk btool props list --debug | grep palo

Check status of fishbucket
/opt/splunk/bin/splunk cmd btprobe -d /opt/splunk/var/lib/splunk/fishbucket/splunk_private_db/ --file /var/log/paloconfig.log


Stop Splunk
/opt/splunk/bin/splunk stop


Clear out fishbucket 
/opt/splunk/bin/splunk cmd btprobe -d /opt/splunk/var/lib/splunk/fishbucket/splunk_private_db/ --file /var/log/paloconfig.log --reset

Check status of fishbucket
/opt/splunk/bin/splunk cmd btprobe -d /opt/splunk/var/lib/splunk/fishbucket/splunk_private_db/ --file /var/log/paloconfig.log

Don't Start Splunk Yet


cat /opt/splunk/etc/apps/studyClub_all_props/default/props.conf

Create a new sourcetype in correct folder
cd /opt/splunk/etc/apps/studyClub_all_props
ll
mkdir local
cd default
ll
cp props.conf ../local/props.conf
cd ../local
ll
vi props.conf
i
:x

Props Settings - Update sourcetype palo_alto_logs to pan:config
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


Update Inputs.conf - the sourcetype for paloconfig.log from palo_alto_logs to pan:config
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

Start Splunk
/opt/splunk/bin/splunk start

Check status of fishbucket
/opt/splunk/bin/splunk cmd btprobe -d /opt/splunk/var/lib/splunk/fishbucket/splunk_private_db/ --file /var/log/paloconfig.log

run search in UI
index=study_club sourcetype=palo_alto_logs source="/var/log/paloconfig.log"


index=study_club source="/var/log/paloconfig.log"
index=study_club source="/var/log/paloconfig.log" sourcetype=palo_alto_logs | delete
index=study_club source="/var/log/paloconfig.log"


head -n 3 /var/log/paloconfig.log
head -n 3 /opt/splunk/var/log/splunk/splunkd.log

see what user splunk is running under
ps -ef | grep splunk
kill -9 <PID> 

