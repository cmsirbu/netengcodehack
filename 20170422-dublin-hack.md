# #netengcode hack @ Facebook Dublin

![Group](https://scontent-ams3-1.xx.fbcdn.net/v/t1.0-9/18275136_10154917053087034_2998705720560180193_n.jpg?oh=c86c67884d24cc72cea5b3972bd7bcb5&oe=59E465CC)

This hack was held on the 22nd of April 2017 at Facebook's Dublin HQ. A few photos are [on the facebook group](https://www.facebook.com/groups/netengcode/photos/) and post-hack write-ups from the participants can be [read here](https://www.trueneutral.eu/2017/netengcodehack-1.html), [here](https://www.linkedin.com/hp/update/6261678832546258944) and [here](https://www.linkedin.com/hp/update/6261669712149639168).

## The Hacks

- [YACT (Yet another CLI tool)](https://github.com/CsilLAB/yact)
- [Remediator](https://github.com/vazic/remediator)
- Network Troubleshooter
- Mapper w/BGP-LS
- [pocketinternet](https://github.com/podomere/pocketinternet)

## All Project Ideas

### Dynamic Network Mapper

This script would do network node and link discovery periodically, creates a connectivity model and then creates a web based dynamic representation of the connections.

Example representation: http://www.alanzucconi.com/2015/11/01/interactive-graphs-in-the-browser/

Requirements: Access to a test network

### LabMan

For efficient use of a test environment it is essential to manage the resources properly.
A user should be able to book a time slot in a booking system. If this is a first time user, all the devices will be configured with a default config. At the end of a time slot, the config is saved. When the user books the next slot, it can be selected which previously saved config must be loaded to the devices.

Requirements: Access to test network

### Naughtyfier - Poor Mans's Policy Engine

Random configuration changes can lead to a network which does not longer align to the standards.
This system would have 2 modules:
1. Administering the changes planned on the network
2. Checks the network for config changes. If the config changed and it's cannot be identified as a planned change, it'll notify a user/group. After a time limit it restores the original config.

Requirements: Access to a test network

### CLIBGP

Thinking out loud here. Feel free to add/comment.

BGP CLI utility to:
- Verify BGP peerings (compliance check)
- Configure/remove peers
- Put device in maintenance (AS prepending, drain traffic)
  - Check whether it's possible or not (too many devices in maintenance state?)
  - Verify if it worked (rollback if it didn't)

I saw a lightning talk at NANOG 68 on something kind of like parts of this - https://github.com/loopodoopo/pms
### gobgp-controller

gobgp is a BGP daemon written in Go. One of the most interesting characteristics of gobgp compared to others is it's grpc interface that allows you to do things like deploy `policies`, `peers` or even manipulate the RIB.

The idea is to write a BGP controller that allows you to operate and verify multiple gobgp instances from a controller via a web UI and REST interface. If someone is interested I put already some scripts together to verify the viability of this project and published them on github (I had 0 experience with gobgp as of 24 hours ago so forgive me if something looks wrong or obvious):

https://github.com/dbarrosop/gobgp-grpc-demo

### Automatic remediation system

System / framework that would receive inputs from network devices (syslog for example) and based on rulesets, take action. For example; if a Linecard produces a critical log, execute an action to disable the bad hardware and notify the operator
- There was a talk on this a while back - https://www.nanog.org/sites/default/files/Swafford_Netops_Coding_201.pdf

### BGP LS based controller

A controller that uses BGP LS to map out the network topology and export it to a system that we can use to for example do centralized traffic steering (PoC could be per hop static routing) to define paths for certain traffic

Some interesting scripts that could help https://github.com/Dipsingh/BGP-LS-Topology-Grapher

### fb_push integration with configuration generation

fbpush is a CLI tool to push config files to Juniper network devices that is able to do dry-runs, rollback, confirmed operations, etc. It can be extended to support other platforms with commit models such as XR based systems.

Idea would be to integrate this tool with other template based configuration systems so the final config could be applied to the device using fbpush
https://github.com/facebook/fbpush

### Another CLI tool

Build a CLI tool to quickly and easily get information from devices in a tabular format. Automatically generation of HTML reports can be an option as well.

It can be really modular (good to work in team). Based on time and human resources it can be expanded in scope (device state validation, vendor agnostic features configuration based on data models, etc.)

It can heavily use NAPALM and it may also ends up into a contribution (http://napalm.readthedocs.io/en/latest/)


### Pocket-Internet

- background: iNOG workshops with easy to provision labs on local machines or IaaS
- idea: infinitely scalable topology (on demand add/remove with automated provisioning) that mimics the Internet, with each participant being an ISP (small mesh of BGP routers that inject prefixes with policy)
- what: blueprints inspired from SP types (transit, IXP, broadband, content) + tasks related to implementing policy between them - implemented with tiny BGP daemons in containers (as lightweight as possible)
- goal: teach BGP concepts + the automation of that brings everything up (can build on it further with network management concepts)

### Ansible - IPAM integration

Develop script that can produce dynamic variables from IPAM (https://github.com/SpriteLink/NIPAP)...
and allocate new prefixes per need using NIPAP's Pools.
This later can be used to dynamically extend network w/o need of manual address
allocation.
UPD: Quick installation howto https://gist.github.com/vazic/d61ebf372655db9c3bc0c119ba8651de

### Omnidisco, a network management collector

https://github.com/aduitsis/snmp-class

Brief presentation: http://www.netmode.ntua.gr/Presentations/Towards%20A%20Scalable%20Network%20Management%20Collector.pdf

Some ideas:
-Possibility to add collection capabilities (i.e. workers) for other protocols (e.g. Prometheus). Add certain capabilities to Fact class, in anticipation of integration with Prometheus.
-Add collection capabilities for other SNMP MIBs or pieces of information
-Remove Master coordination daemon from architecture and allow workers to writeback to Redis datastore directly.

note: This is Modern Perl using a lot of libraries, plus Redis, plus ElasticSearch (optional), plus Gearman. So it might be time-consuming just to setup from scratch. And there may be a shortage of Perl folk :). However, inserting this anyway and we'll see.



