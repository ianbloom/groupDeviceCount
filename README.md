# groupDeviceCount
## Introduction
The Group Device Count DataSource utilizes the LogicMonitor API to query device group folders for their respective device
counts.  These device group folders exist as instances.  The DataSource then uses the API to update instance level alert
thresholds to trigger alerts based on a user defined device limit, which is defined as a device group level property.

## Setup
### Device Properties
In order to reduce the number of API calls made during collection, it is recommended that this DataSource be applied to a 
**SINGLE** device.  The user must define the following device properties for the device that the DataSource is applied to:

* lmaccess.id
* lmaccess.key
* lmaccount

The lmaccess.id and lmaccess.key correspond to a user's API credentials. The lmaccount refers to the ### in a customer's 
###.logicmonitor.com URL.

### Group Level Properties
In addition, the user must apply the following group level property to device groups they would like to monitor:

* device_limit

This property is used to dynamically update the instance level alert threshhold for the "device_count" datapoint of the
corresponding instance.  If device_limit is set to 0, then the instance will not alert on "device_count".

## How It Works
The Active Discovery script of this DataSource queries the LogicMonitor API for a list of device groups, if the script finds
a device group with the "device_limit" property, the script creates a dictionary with this device groups name, id, 
device_limit, and deviceDatasourceId (A unique ID assigned to all device/DataSource pairs).  These become the instance name, 
ID, and respective instance level properties.

The Collector script queries the API for the device count of each instance.  The "device_limit" property of each group (which 
is now an instance level property) is used to dynamically update the alert threshold for each respective instance.  This 
DataSource currently triggers a warning level alert if the threshold is exceeded.
