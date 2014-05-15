node-ams-sdk
============

* Provides lightweight wrapper around Azure Media Services REST API


### Usage 


```
//Eventually...

npm install --save node-ams-sdk

```

You initialize the service by providing a configuration object

```
var AzureService  = require('node-ams-sdk')
var serviceConfig = require('../path/to/config') || {your: config}

var amsService = new AzureService(serviceConfig)

```

If you already have your ams token, then you can specify it in your config like

```
{
    access_token: "exampletokengoeshere",
    token_expires: theDateItExpires
}
```

If you do not have this, then you will need to supply the "client_id" and "client_secrect" in the config. Before you make subsequent requests, you will need to set the access token by calling the method "setToken"

```
amsService.setToken( function (err) {
    //check for error
    //do other stuff. no response returned.
}
```
Calling this will set the token as in internal property to be used on subsequent requests.

### Azure Media Services Resources Provided

#### Assets
-----------

There are 5 methods that you can call on an assets resource. Internally, they will make the requests to Azure Media Services and return to you the response

**listAssets([query],[cb])**

Takes an optional query and optional callback

If a callback is provided, it will return an ```( err, response )``` signature. If no callback is provided, it will return a stream.


**getAsset(assetId, [query],[cb])**

Takes a required assetId and an optional query and optional callback.

If a callback is provided, it will return an ```( err, response )``` signature. If no callback is provided, it will return a stream.

**createAsset([data], cb)**

Data is and optional parameter. Asset can be created without data. It will just create an asset of Name=null. The allowable keys are

```
Name
AlternateId
ContentKeys
ParentAssets
Options
StorageAccountName
```

*AlternateId should be a string*

A callback is required. No streaming out of a create.

**updateAsset(assetId, data, cb)**

Requires an assetId, a data object, and a callback. This operation will preform a MERGE. Any parameter provided will overwrite underlying object.

```
Name
AlternateId
ContentKeys
ParentAssets
```

*AlternateId should be a string*

A callback is required. No streaming out of a create.

**removeAsset(assetId, cb)**

Requires an assetId and a callback. No streaming.

**getAssetMetadata(assetId, [cb])**

Requires an assetId. Callback is optional. Will stream without callback. Will link up the data that is in blob storage to the supplied asset. If it works, this will return a status code of 204 with no data.


#### Access Policies
---------------------

**listAccessPolicies([cb])**

Takes an optional callback. Will list all access policies - streaming if no cb.

**listAssetAccessPolicies(assetId, [cb])**

Requires an assetId, Optional callback. Will list all access policies for a given asset - streaming if no cb.

**getAccessPolicy(accessPolicyId, [cb])**

Requires an accessPolicyId, Optional callback. Will return details about selected access policy - streaming if no cb.

**createAccessPolicy(data, cb)**

Requires data and callback. Will create a new access policy. Allowed data properties are:

```
Name
DurationInMinutes
Permissions
```

**removeAccessPolicy(accessPolicyId, cb)**

Requires accessPolicyId, and callback. Will remove existing access policy.

#### Locators
-------------

**listLocators([cb])**

Takes an optional callback. Will list all locators - streaming if no cb.

**listAssetLocators(assetId, [cb])**

Requires an assetId, Optional callback. Will list all locators for a given asset - streaming if no cb.

**getLocator(locatorId, [cb])**

Requires an locatorId, Optional callback. Will return details about selected locator - streaming if no cb.

**createLocator(data, cb)**

Requires data and callback. Will create a new locator. Allowed data properties are:

```
AccessPolicyId
AssetId
StartTime (Format: 'MM/DD/YYYY hh:mm:ss A')
Type
Name
ExpirationDateTime (Format: 'MM/DD/YYYY hh:mm:ss A')
```

**updateLocator(locatorId, data, cb)**

Requires locatorId, data, and callback. Will upate an exisiting locator. Allowed data properties are:

```
StartTime (Format: 'MM/DD/YYYY hh:mm:ss A')
Name
ExpirationDateTime (Format: 'MM/DD/YYYY hh:mm:ss A')
```

**removeLocator(locatorId, cb)**

Requires a locatorId and a callback. Will remove selected locator

#### Encoding Job
-----------------

**createEncodingJob(options, cb)**

Requires an options object and a callback. Will create an encoding job for given asset. Options are as follows:

```
name       - The name of the job
assetId    - The id of the asset to be encoded
encoding   - String representation of encoding e.g. "H264 Broadband 720p"
outputName - The name of the output asset
```

**getJobStatus(jobId, [cb])**

Requires a jobId. Will return the status of the job. Will stream is optional callback is not provided