## Core Infrastructure

### Indexer
---
- Instance that indexes data
- Transform raw data into events
- Place results into an index
- Searches the indexed data in response to search requests
- Store data and supply it in response to search reqs from search heads
- cpu and disk i/o intensive
#### Recommendations:
- Min. 12 cores/12gb ram
- 800-1200 (I/O) IOPS

### Search Head
---
- Handles search management and coordinates across multiple indexers
- Are how users primarily interact with splunk.
- This is where dashboards, visualizations, lives in.
 
## Supporting Infrastructure

### Forwarders
---
- Forwards data to another spluk instance such as: indexer,another fwdr, thirdy-party system
- 02 types of fwdrs:
 1) Universal Forwarder (UF):
	- Dedicated, streamlined vers of splunk enterprise
	  , contains essential components needed to send data to another Splunk Enterprise instance or to a thirdy-party system.
	- Best way for sending data to indexers
	- **Main limitation** is that it fwdrs only unparsed data.
	- Does not support python neither expose a UI.
	- Must use a heavy fwrdr to route event-based data.
	- Typically, you will install these on any WIndows/Linux system you would like collet logs.
2) Intermediate forwarder (IF) (heavy forwarder):
	- Full splunk enterpr installed. w/ features disabled to achieve 
	  smaller footprint.

### Syslog Receiver
---
This is a way to getting data into splunk from devices don't support running universal forwarder (UF) such as netwrok devices like fw, routers or sw.
Splunk can receive direct TCP/UDP inputs.

The best practice is "never do this", because you will have to restart splunk sometimes,for install an app or change configuration, those UDP/TCP port are not open anymore and you can lose data.

For receiving syslog, you have to deploy syslog-ng on a heavy/universal fwdr.
Syslog-ng will receive incoming syslog, and write logs to `/var/log/network/<sourcetype>/<host>/syslog.log.date`.

Splunk inputs will be config to read this file.

### Deployment Server
---
- Splunk enterprise instance, acts as a centralized config manager.
- Group together and collectively manage any number of splunk instances.
- Instances which are remotely config by **deployment srvs** are called _deployment clients_.
- Deployment srvs **downloads updated content**, such  as config files/apps to deplyment clients.

### Deployment client
---
- Splunk instance remotely configured by a deplyment server.
- Can be grouped togeter into one/more servre classes.
- Both, Splunk Enterprise/Splunk UF can be deplyment clients.
- If deployment srv has new content for client srv class, it distriutes to the polling client.

### Deployment App
---
- Unit of content deployed by the deployment srv to a group of deployment clients.
- Deployment apps can be fully developed apps similar than which are in _Splunkbase_, or they can be simple groups of confgs.
- It -s recommended use apps as "building blocks" and them combine more of them to achieve your goal.

### Server Classes
---
- Is a group of deployment clients ans associated apps.
- They facilitates the mgmt of a set of deployments clients  as a single unit.
- Can group deployment client by application, PS,data type to be indexed or
- any feature of a splunk enterprise deployment.
- Is used by a deplyment server to determine what content to deply to groups of deployment clients.
- The fwdr mgt interfce offers an easy way to creae,edit, mng server classes.


## Splunk Licensing
---
- SPlunk is licensed by the amount of data ingested per day.
- The license server typically runs on the "_deployment server_"
