---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - curl

toc_footers:
  - <a href='https://chaldal.com'>Chaldal.com</a>
  - Written by <a href="mailto:abdullah.al.jahid@gmail.com"> Abdullah Al Jahid</a>
  - Senior Software Engineer
  - Chaldal.com

includes:

search: true
---

# Intruduction

#### API ENDPOINTS
* Live: [http://eggtransport.chaldal.com/api](http://eggtransport.chaldal.com/api) 


API  Explorer available at - ( Powered by Swagger )

* Live: [http://eggtransport.chaldal.com/swagger](http://eggtransport.chaldal.com/swagger) 


# Authentication
```
X-Egg-ApiKey : 00000000-0000-0000-0000-000000000000
X-Egg-UserId : 00000000-0000-0000-0000-000000000000
```
Authentication are done by simple http header based Authentication. Authenticate using the API by including your secret API key and User Id in the request. If you already don't have them - contact us at tech@chaldal.com to get one.

Your API key and User Id carry many privileges, so be sure to keep them secret! Do not share your secret API keys in publicly accessible areas such GitHub, client-side code, and so forth.

You will need to pass your User Id and API key as http header request to all the API requests. API key as `X-Egg-ApiKey` and User ID as `X-Egg-UserId`.


# Parcel


## Parcel Object

```json
{
  "TrackingRef":"IA1B18000",
  "MerchantRef":"S171008002",
  "Physique": {
      "$Case":"Tangible",
      "Dimension": {
          "$Case":"Partial",
          "MaxDimension":10,
          "Volume":1000
      },
      "Mass":{ 
          "$Case":"Partial",
          "Total":1000
      },
      "RequiresRefrigeration":false
  },
  "AddressInfo":{
      "Address":410000,
      "Identity":"00000000-0000-0000-0000-000000000000",
      "FullName":"CUSTOMER NAME",
      "StreetAddress":"Niketon Gulshan",
      "PhoneNumber":"+8801900000000",
      "DeliveryArea":{
          "Id":24,
          "Name":"Niketan"
      },
      "CustomerHub":"Banani",
      "GeoLocation":null,
      "ConfidenceLevel":"Level0"
  },
  "SlotStart":"2017-10-11T11:30:00+06:00",
  "SlotEnd":"2017-10-11T15:00:00+06:00",
  "CashToCollect":0,
  "StockValue":0,
  "Weight":"Less Than 1 Kg",
  "Status":"Delivered",
  "TaskType":"Delivery",
  "MerchantHub":"Mirpur"
}
```

This object represent a parcel. In the API responses its refered as "Task". 

| ATTRIBUTES  | Description                                           | type   |
|-------------|-------------------------------------------------------|--------|
| TrackingRef | The system generated unique tracking key for a parcel | string |
| MerchantRef | Marchant defined reference key.                       | string |



## Search Parcels

> Definition

```
GET http://eggtransport.chaldal.com/api/Task/SearchTasksForMyOrg
```

> Example Request

```shell
# Make sure to replace `X-Egg-ApiKey` and `X-Egg-UserId` with your API key.
$ curl 'http://eggtransport.chaldal.com/api/Task/SearchTasksForMyOrg?searchText=&taskType=All&highLevelStatus=All&fromDate=null&toDate=null&pageNum=1&pageSize=20' \
  -X GET \
  --header 'Accept: application/json' \
  --header 'X-Egg-ApiKey : 00000000-0000-0000-0000-000000000000' \
  --header 'X-Egg-UserId : 00000000-0000-0000-0000-000000000000'
```
> Example Response

```json
{
    Data: [
        {
            "TrackingRef":"IA1B18000",
            "MerchantRef":"S171008002",
            "Physique": {
                "$Case":"Tangible",
                "Dimension": {
                    "$Case":"Partial",
                    "MaxDimension":10,
                    "Volume":1000
                },
                "Mass":{ 
                    "$Case":"Partial",
                    "Total":1000
                },
                "RequiresRefrigeration":false
            },
            "AddressInfo":{
                "Address":410000,
                "Identity":"00000000-0000-0000-0000-000000000000",
                "FullName":"CUSTOMER NAME",
                "StreetAddress":"Niketon Gulshan",
                "PhoneNumber":"+8801900000000",
                "DeliveryArea":{
                    "Id":24,
                    "Name":"Niketan"
                },
                "CustomerHub":"Banani",
                "GeoLocation":null,
                "ConfidenceLevel":"Level0"
            },
            "SlotStart":"2017-10-11T11:30:00+06:00",
            "SlotEnd":"2017-10-11T15:00:00+06:00",
            "CashToCollect":0,
            "StockValue":0,
            "Weight":"Less Than 1 Kg",
            "Status":"Delivered",
            "TaskType":"Delivery",
            "MerchantHub":"Mirpur"
        }
    ],
    TotalCount: 57
}

```

`GET /Task/SearchTasksForMyOrg`


Listing and searching parcels are done through this single API endpoint. 
If you just want to list all latest parcels - keep most of the searching params empty.
Otherwise use available query params to filter/search your specific parcel.

### URL Parameters
| Parameter       | Description                                                                                                                                                                | Parameter Type | Data Type |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|-----------|
| searchText      | Search `TrackingRef`, `MerchantRef`, associated address `PhoneNumber`.  It will match all of these fields and return if anything. If no search needed pass a empty string  | query          | string    |
| taskType        | Available values are - `All`, `Delivery`, `PickUp`                                                                                                                         | query          | string    |
| fromDate        | Self explanatory. Pass `null` if you want to ignore this field. Example date format - 2017-09-08T00:00:00+06:00. Make sure its URI encoded                                 | query          | date-time |
| toDate          | Similar to `fromDate`                                                                                                                                                      | query          | date-time |
| highLevelStatus | Filter by high level status. Available options are - `All`, `Preparing`, `Delivering`, `Returning`, `Completed`, `Failed`                                                  | query          | string    |
| pageNum         | For pagination, Page number                                                                                                                                                | query          | integer   |
| pageSize        | Number of items to show in a single page                                                                                                                                   | query          | integer   |





## Get Parcels

> Definition
 
```
POST http://eggtransport.chaldal.com/api/Task/GetTasks
```

> Example Request

```shell
# Make sure to replace `X-Egg-ApiKey` and `X-Egg-UserId` with your API key.
$ curl 'http://eggtransport.chaldal.com/api/Task/GetTasks' \
 -X POST \
 --header 'Accept: application/json' \
 --header 'Content-Type: application/json' \
 --header 'X-Egg-ApiKey : 00000000-0000-0000-0000-000000000000' \
 --header 'X-Egg-UserId : 00000000-0000-0000-0000-000000000000' \
 -d '["YMMP68CNJ","VHM1W8CNL"]' 
```

> Example Response

```json
{
  "VHM1W8CNL": {
    "TrackingRef": "VHM1W8CNL",
    "MerchantRef": "Ombre by Ash-R1J284",
    "Physique": {
      "$Case": "Tangible",
      "Dimension": {
        "$Case": "Unavailable"
      },
      "Mass": {
        "$Case": "Unavailable"
      },
      "RequiresRefrigeration": false
    },
    "AddressInfo": {
      "Address": 415409,
      "Identity": "00000000-0000-0000-0000-000000000000",
      "FullName": "Ayesha Marzana",
      "StreetAddress": "House 60/D .Road 131. Gulshan 1,Vantage Travel Agency-2nd floor.,",
      "PhoneNumber": "+8801630167640",
      "DeliveryArea": {
        "Id": 15,
        "Name": "Gulshan"
      },
      "CustomerHub": "Banani",
      "GeoLocation": null,
      "ConfidenceLevel": "Level0"
    },
    "SlotStart": "2017-10-11T11:30:00+06:00",
    "SlotEnd": "2017-10-11T15:00:00+06:00",
    "CashToCollect": 1310,
    "StockValue": 1310,
    "Weight": "Unknown",
    "Status": "Delivering",
    "TaskType": "Delivery",
    "MerchantHub": "Kallayanpur"
  },
}
```

`POST /Task/GetTasks`

Get a specific parcel by providing the parcel tracking reference number. 
#### Parameter 
| Parameter       | Description                                                                                                                                                               | Parameter Type | Data Type        |
|-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|------------------|
| trackingRef     | Just pass an array of tracking reference numbers. You can pass multiple tracking numbers. To get only one- just pass a single item inside the array, ex - ['CD043DD83']   | body           | Array[string]    |



---


## Create Delivery Parcel

> Definition
 
```
POST http://eggtransport.chaldal.com/api/Task/CreateThirdPartyDeliveryTask
```


`POST /Task/CreateThirdPartyDeliveryTask`


Create normal delivery parcel



## Create Pickup Parcel

> Definition
 
```
POST http://eggtransport.chaldal.com/api/Task/CreateThirdPartyPickUpTask
```


`POST /Task/CreateThirdPartyPickUpTask`


Create pickup parcel



