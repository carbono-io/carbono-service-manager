# Service Manager

This package purpose is to register a service in 
[etcd](https://github.com/coreos/etcd) and retrive the service's needed hosts 
to operate. It relies on [config](https://www.npmjs.com/package/config) to get the data needed to run, if not 
present the process will exit.

## Install

    $ npm install carbono-service-manager

## Usage

Server `index.js`:
```
	var server = app.listen(80, function () {
    	// Service discovery and registration
    	require('carbono-service-manager');
	});
```

Config `default.json` file:
```
	{
	    "etcd": {
			"host": "192.168.0.69",
			"port": 7689,
			"key" : "cbs1", // The service key in etcd
			"alias" : "Carbono Service 1",
			"dependencies" : [
				{"key" : "cbs6", "alias" : "Carbono Service 6"},
				{"key" : "cbs12", "alias" : "Carbono Service 12"},
				{"key" : "cbs2", "alias" : "Carbono Service 2"},
				{"key" : "cbs7", "alias" : "Carbono Service 7"}
			]
    }
```

Server `service-6-client.js` module:
```
    var etcd = require('carbono-service-manager');
	var hostAddress = etcd.getServiceUrl('cbs6');
```

In Output:
```
	+ Module Carbono Service 1 registered with etcd.
	- Service Carbono Service 6 found at etcd.
	- Service Carbono Service 12 found at etcd.
	- Service Carbono Service 2 found at etcd.
	- Service Carbono Service 7 found at etcd.
```

In case a service is not found or the module is unable to register itself the process will exit.

```
	+ Module Carbono Service 1 registered with etcd.
	[ERROR] Finding Carbono Service 6 with etcd: {"errorCode":100,"message":"Key not found","cause":"","index":141}
```