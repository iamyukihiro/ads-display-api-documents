# Error code
List of error code and message when the error or problem occurs.

### Overview
If your request is successful, the Display Ads API will return an HTTP 200 OK response code and the response. <br> If an error occurs during the processing of your request, Display Ads API will return an error code and message. <br>See [Error](/docs/en/api_reference/data/Common/Error.md), [ErrorDetail](/docs/en/api_reference/data/Common/ErrorDetail.md) for the entity details

### Sample Error Response

There are 3 types of error response, HTTP status, Full error and Part error. See the following description about each response.

#### HTTP status

HTTP status response does not returns "Status: 200 OK" for request syntax violation, system error and authentication error.<br>
For example, if access token authentication fails, HTTP status response returns "Status: 401 Unauthorized" and message with error codes.

```json
{
  "accountIds": [
    11111111
  ],
  "numberResults": 0,
  "startIndex": 1
}
```

```json
{
    "rid": "178ed42a0e8f26acbb37ac2da2969bdd",
    "errors": [
        {
            "code": "0111",
            "message": "Authentication failed.",
            "details": []
        }
    ]
}
```
※Look API reference for other HTTP Status.

#### Full Error

Full Error is returned with response code "Status: 200 OK" for the fail of entire request caused by request constraints.<br>
The following response is an example of Full Error in the case of invalid ‘numberResults’ value of `Paging` in AccountService:get.

##### <Request Sample>
```json
{
  "accountIds": [
    11111111
  ],
  "numberResults": 0,
  "startIndex": 1
}
```
##### <Response Sample>
```json
{
    "rid": "02505e66279a0a103f05421a6529ac7b",
    "errors": [
        {
            "code": "0001",
            "message": "Invalid Request.",
            "details": [
                {
                    "requestKey": "numberResults",
                    "requestValue": "must be greater than or equal to 1"
                }
            ]
        }
    ]
}
```

#### Part Error

Part Error is returned in the error with "Status: 200 OK" case caused by constraints of each element in ‘<operand>’.<br>
Part Error is returned for each ‘<operand>’, whether an error occurred or not.<br>
The error response may contain both of ‘true’ and ‘false’ as value of ‘<operationSucceeded>’, since '<value>' returns for each '<operand>' when you send a single request which includes multiple ‘<operand>’.<br>
<br>
The following response an example of a request in the case include normal operand and error operand in ReportDefinitionService:add.

##### <Request Sample>
```json
{
  "accountId": 11111111,
  "operand": [
    {
      "dateRangeType": "TODAY",
      "fields": [
        "COST",
        "IMPS"
      ],
      "lang": "EN",
      "reportName": "ex_report_01"
    },
    {
      "dateRange": {
        "endDate": "20190901",
        "startDate": "20190930"
      },
      "dateRangeType": "CUSTOM_DATE",
      "fields": [
        "COST",
        "IMPS"
      ],
      "lang": "JA",
      "reportName": "ex_report_02"
    }
  ]
}
```
##### <Response Sample>
```json
{
    "errors": null,
    "rid": "b3402cb66f05977f9deebfb39eaaf345",
    "rval": {
        "values": [
            {
                "errors": null,
                "operationSucceeded": true,
                "reportDefinition": {
                    "accountId": 11111111,
                    "completeTime": null,
                    "dateRange": null,
                    "dateRangeType": "TODAY",
                    "downloadEncode": "UTF-8",
                    "downloadFormat": "CSV",
                    "fields": [
                        "COST",
                        "IMPS"
                    ],
                    "filters": null,
                    "frequencyRange": null,
                    "jobStatus": "ACCEPTED",
                    "lang": "EN",
                    "reportJobErrorDetail": null,
                    "reportJobId": 22222222,
                    "reportName": "ex_report_01",
                    "requestTime": "20200721135246",
                    "sortFields": null,
                    "zip": "OFF"
                }
            },
            {
                "errors": [
                    {
                        "code": "V0001",
                        "message": "Invalid value.",
                        "details": [
                            {
                                "requestKey": "dateRange.startDate",
                                "requestValue": "20190930"
                            },
                            {
                                "requestKey": "dateRange.endDate",
                                "requestValue": "20190901"
                            }
                        ]
                    }
                ],
                "operationSucceeded": false,
                "reportDefinition": null
            }
        ]
    }
}
```

### Common Error Codes of All Service
Display Ads API returns these HTTP status when errors occur.

 Code    | Message        | Description
-------- | -------------- | ------------------------
F0001  | Invalid format.   | The value format is invalid.
R0001 | Require. | It is missing required parameter.
V0001 | Invalid value. | The value is invalid.
L0001 | Lower list size. | The number of elements in the array is below the lower limit.
L0002 | Over list size. | The number of elements in the array exceeds the upper limit.
U0002 | Invalid url. | The upload URL or download URL is invalid.
S0001 | Invalid Status. | The status is invalid.
I0001 | Deactivated Id. | The ID is Deactivated.
D0001 | Duplicate. | The unique value is duplicated.
RL001 | Invalid relation. | The relation of the request is contradictory.<br> For example, you are specifying the date of start > end.
LT001 | Over limit. | The upper limit value that can be registered is exceeded.
0001 | Invalid Request. | This error can result for a variety of reasons. <br>Typically because one of the parameter values in the request is wrong or invalid and the operation cannot be completed.
0002 | An internal error has occurred. | An internal error has occurred. Please try again later. <br>If the problem continues, please contact the support team via Inquiry page for assistance.
0003 | Frequency limit exceeded. Please try your request again later. | Frequency limit exceeded. Please try your request again later.
0004 | URL not found. | URL not found.
0005 | Bad request. | Bad request.
0098 | Permission denied. | Permission denied.
0099 | Out of service. | API is under maintenance or not available to use.

※Look [API reference](http://ads-developers.yahoo.co.jp/reference/ads-display-api/?lang=en) for error of each Service.
