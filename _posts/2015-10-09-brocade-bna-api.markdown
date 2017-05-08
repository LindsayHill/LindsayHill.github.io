---
author: lindsay
date: 2015-10-09 01:33:07+00:00
layout: post
slug: brocade-bna-api
title: Brocade BNA API
categories:
- NMS
tags:
- BNA
- Brocade
---

[Brocade Network Advisor](http://www.brocade.com/en/products-services/network-management/brocade-network-advisor.html) (BNA) has a [REST API](http://www1.brocade.com/downloads/documents/html_product_manuals/NA_1240_RESTAPI/wwhelp/wwhimpl/js/html/wwhelp.htm) for accessing Fibre Channel-related data. The documentation includes a sample Python script showing how to connect to the API to retrieve Fabric info. The script given only works with Python 3.x. It's also a pain to copy out of the documentation as you end up with a few extra characters in there. Here's a version that will work with Python 2.7. I've also made a few other modifications - in this one, you can set the BNA IP, Username & Password at the top of the script.  I've also made it PEP8-compliant.


```python
#!/usr/bin/env python

import httplib
import json
import sys

BNAServer = "10.200.5.181"
BNAUsername = "Administrator"
BNAPassword = "password"

# Create HTTPConnection object and connect to the server.
connection = httplib.HTTPConnection(BNAServer)

###########################
# Log in to Network Advisor
###########################

# Send login request
connection.request(
    'POST',
    '/rest/login',
    headers={
        "WSUsername": BNAUsername,
        "WSPassword": BNAPassword,
        "Accept": "application/vnd.brocade.networkadvisor+json;version=v1"}
    )

print()
print("Sending login request to Network Advisor...")

# Get the response
response = connection.getresponse()
# Display the response status print()
print ("Status= ", response.status)
# If successful (status = 200), display the returned session token
if response.status == 200:
    WStoken = response.getheader('WStoken')
    print()
    print("Login successful!")
    print("WStoken: ", WStoken)
else:
    print()
    print (response.status, response.reason)

connection.close()

###########################
# Retrieve fabrics
###########################

# Send GET request
connection.connect()
connection.request(
    'GET',
    '/rest/resourcegroups/All/fcfabrics',
    headers={
        "WStoken": WStoken,
        "Accept": "application/vnd.brocade.networkadvisor+json;version=v1"}
    )

print()
print("--------------------------------------------------------------------")
print("Getting list of all fabrics...")

# Get the response
response = connection.getresponse()

# Display the response status
print()
print ("Status= ", response.status)

# If successful (status = 200), display the returned list in JSON format
if response.status == 200:
    print()
    print("List of fabrics:")
    json_response_bytes = response.read()
    json_response_string = json_response_bytes.decode('utf-8')
    list_of_fabrics_dict = json.loads(json_response_string)
    print(json.dumps(list_of_fabrics_dict, indent=4))
    print("Number of FC fabrics: ", len(list_of_fabrics_dict["fcFabrics"]))
else:
    print()
    print (response.status, response.reason)
    sys.exit()

connection.close()

##############################
# Retrieve details of a fabric
##############################

# Get the key of the first fabric in the list
fabric_key = list_of_fabrics_dict["fcFabrics"][0]["key"]

# Send GET requrest
connection.connect()
connection.request(
    'GET',
    '/rest/resourcegroups/All/fcfabrics/'+fabric_key,
    headers={
        "WStoken": WStoken,
        "Accept": "application/vnd.brocade.networkadvisor+json;version=v1"}
    )

print()
print("--------------------------------------------------------------------")
print("Get fabric "+fabric_key+" details...")

# Get the response
response = connection.getresponse()

# Display the response status
print()
print ("Status= ", response.status)

# If successful (status = 200), display the returned list in JSON format
if response.status == 200:
    print()
    print("Fabric details:")
    json_response_bytes = response.read()
    json_response_string = json_response_bytes.decode('utf-8')
    fabric_details_dict = json.loads(json_response_string)
    print(json.dumps(fabric_details_dict, indent=4))
else:
    print()
    print (response.status, response.reason)
    sys.exit()

connection.close()

######################################################
# Retrieve list of switches in the context of a fabric
######################################################

# Send GET requrest
connection.connect()
connection.request(
    'GET',
    '/rest/resourcegroups/All/fcfabrics/'+fabric_key+'/fcswitches',
    headers={
        "WStoken": WStoken,
        "Accept": "application/vnd.brocade.networkadvisor+json;version=v1"}
    )

print()
print("--------------------------------------------------------------------")
print("Get the list of switches under fabric "+fabric_key+" ...")

# Get the response
response = connection.getresponse()

# Display the response status
print()
print ("Status= ", response.status)

# If successful (status = 200), display the returned list in JSON format
if response.status == 200:
    print()
    print("List of switches:")
    json_response_bytes = response.read()
    json_response_string = json_response_bytes.decode('utf-8')
    list_of_fabric_switches_dict = json.loads(json_response_string)
    print(json.dumps(list_of_fabric_switches_dict, indent=4))
else:
    print()
    print (response.status, response.reason)
    sys.exit()

connection.close()

```


Save that, edit the server IP & username/password, then run it. You should get output that looks something like this:


```text
bna-api lhill$ ./na_client.py
()
Sending login request to Network Advisor...
('Status= ', 200)
()
Login successful!
('WStoken: ', '3CSkxp2i4HPtAiQFOLY/ha6mF7w=')
()
--------------------------------------------------------------------
Getting list of all fabrics...
()
('Status= ', 200)
()
List of fabrics:
{
    "fcFabrics": [
        {
            "fabricName": "",
            "name": "Fabric B",
            "seedSwitchWwn": "10:00:00:05:33:A3:DF:90",
            "description": null,
            "principalSwitchWwn": "10:00:00:05:33:A3:DF:90",
            "virtualFabricId": 128,
            "contact": null,
            "location": null,
            "key": "10:00:00:05:33:A3:DF:90",
            "seedSwitchIpAddress": "10.200.10.100",
            "adEnvironment": false,
            "secure": false
        },
        {
            "fabricName": "",
            "name": "Fabric A",
            "seedSwitchWwn": "10:00:00:05:1E:92:D8:00",
            "description": null,
            "principalSwitchWwn": "10:00:00:05:33:8A:7C:6A",
            "virtualFabricId": -1,
            "contact": null,
            "location": null,
            "key": "10:00:00:05:33:8A:7C:6A",
            "seedSwitchIpAddress": "10.200.7.110",
            "adEnvironment": false,
            "secure": false
        }
    ],
    "totalResults": null,
    "startIndex": null,
    "itemsPerPage": null
}
('Number of FC fabrics: ', 2)
()
--------------------------------------------------------------------
Get fabric 10:00:00:05:33:A3:DF:90 details...
()
('Status= ', 200)
()
Fabric details:
{
    "fcFabrics": [
        {
            "fabricName": "",
            "name": "Fabric B",
            "seedSwitchWwn": "10:00:00:05:33:A3:DF:90",
            "description": null,
            "principalSwitchWwn": "10:00:00:05:33:A3:DF:90",
            "virtualFabricId": 128,
            "contact": null,
            "location": null,
            "key": "10:00:00:05:33:A3:DF:90",
            "seedSwitchIpAddress": "10.200.10.100",
            "adEnvironment": false,
            "secure": false
        }
    ],
    "totalResults": null,
    "startIndex": null,
    "itemsPerPage": null
}
()
--------------------------------------------------------------------
Get the list of switches under fabric 10:00:00:05:33:A3:DF:90 ...
()
('Status= ', 200)
()
List of switches:
{
    "totalResults": null,
    "startIndex": null,
    "fcSwitches": [
        {
            "baseSwitch": false,
            "domainId": 160,
            "adCapable": false,
            "name": "fcr_fd_160",
            "portBasedRoutingPresent": false,
            "defaultLogicalSwitch": false,
            "inOrderDeliveryCapable": false,
            "dynamicLoadSharingCapable": false,
            "state": "UNKNOWN",
            "statusReason": null,
            "autoSnmpEnabled": true,
            "virtualFabricId": -1,
            "operationalStatus": "HEALTHY",
            "role": "SUBORDINATE",
            "fmsMode": false,
            "key": "50:00:51:E9:2D:80:0E:0A",
            "fcsRole": "None",
            "lfEnabled": false,
            "wwn": "50:00:51:E9:2D:80:0E:0A",
            "type": 40,
            "persistentDidEnabled": false
        },
        {
            "baseSwitch": false,
            "domainId": 2,
            "adCapable": true,
            "name": "SW6510-B",
            "portBasedRoutingPresent": false,
            "defaultLogicalSwitch": true,
            "inOrderDeliveryCapable": false,
            "dynamicLoadSharingCapable": true,
            "state": "ONLINE",
            "statusReason": "Switch Status is HEALTHY. Contributors:",
            "autoSnmpEnabled": true,
            "virtualFabricId": 128,
            "operationalStatus": "HEALTHY",
            "role": "PRINCIPAL",
            "fmsMode": false,
            "key": "10:00:00:05:33:A3:DF:90",
            "fcsRole": "None",
            "lfEnabled": false,
            "wwn": "10:00:00:05:33:A3:DF:90",
            "type": 109,
            "persistentDidEnabled": false
        },
        {
            "baseSwitch": false,
            "domainId": 1,
            "adCapable": true,
            "name": "DCX2",
            "portBasedRoutingPresent": false,
            "defaultLogicalSwitch": true,
            "inOrderDeliveryCapable": false,
            "dynamicLoadSharingCapable": true,
            "state": "ONLINE",
            "statusReason": "Switch Status is HEALTHY. Contributors:",
            "autoSnmpEnabled": true,
            "virtualFabricId": -1,
            "operationalStatus": "HEALTHY",
            "role": "SUBORDINATE",
            "fmsMode": false,
            "key": "10:00:00:05:1E:75:4A:00",
            "fcsRole": "None",
            "lfEnabled": false,
            "wwn": "10:00:00:05:1E:75:4A:00",
            "type": 77,
            "persistentDidEnabled": false
        }
    ],
    "itemsPerPage": null
}
```


Hope this helps someone trying to get started with the API.
