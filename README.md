# rrrelay

Relay HTTP POST transponder passings from Race Result Decoder
and Track Box units to telegraph as timer messages:

	INDEX;SOURCE;CHANNEL;REFID;TOD;DATE

   - INDEX : passing sequence number (set by the relay)
   - SOURCE : decoder ID or nickname
   - CHANNEL : loop id or timing channel
   - REFID : transponder unique ID
   - TOD : local time of day string eg 13h27:52.4321
   - DATE : date of passing  eg 2023-02-27


## HTTP Endpoints

   - POST /rrs : Race Result decoder (passive and active)
   - POST /tbp : Track Box Ping
   - POST /tbs : Track Box Status


## Configuration

Configuration is via metarace sysconf section 'rrrelay' with the
following keys:

key		|	(type) Description [default]
---		|	---
port		|	(int) HTTP listen port [53037]
passtopic	|	(string) MQTT relay topic for passings ['timing/data']
statustopic	|	(string) MQTT relay topic for status ['timing/status']
qos		|	(int) qos value for passing messages [2]
userid		|	(string) optional RR customer ID ['']
passiveloop	|	(dict) map passive device ids to loop ids [{}] (1)

Notes:

   1. Decoders reporting passive transponder reads can have their loopid
      overridden to force a specific channel id. For example, the following
      entry for passiveloop would assign all passive reads from decoder
      "D-23678" channel id "C5".

	"passiveloop":{"D-23678":"C5"}


## Decoder Setup

### Race Result System

Open the decoder web interface and select the "Configuration" link.
Under "Upload target" select the option "Custom (HTTP)",
and enter the public ip, port and path to your rrrelay instance:

![RRS Config](rrs_config.png "RRS Config")

Then enable mobile upload on the decoder panel menu.

### Track Box

## Requirements

   - metarace >=2.0

## Installation

Install with pip:

	$ pip3 install rrrelay

