# HAPI Specification 
Version 1.0 
Heliophysics Data and Model Consortium (HDMC) 
July 26, 2016 

# Introduction
This document describes the HDMC Application Programmer’s Interface (HAPI) specification for a set of services to enable access to digital Heliophysics time series data. The focus of HAPI is to provide a uniform way for large or small data providers to expose one or more data holdings in an interoperable way. The interface methods required to be a HAPI-compliant server comprise a minimum but complete set of capabilities needed to provide access to digital time series data, with a particular focus on uniform access to digital content, and less of a focus on data discovery. The HAPI specification is intended to be simple enough to be added as an additional access pathway by an existing, large-scale data provider. For small data providers who are currently exposing data only as files in an FTP or HTTP site, the HAPI specification is simple enough that it could be implemented as a primary access mechanism, allowing small provider data holdings to participate in a larger data environment. A uniform access layer to many data holdings can then become a foundation for building more interoperable data services.

The API is built using REST principles that emphasize URLs as stable endpoints through which clients can request data. Because it is based on well-established HTTP request and response rules, a wide range of HTTP clients can be used to interact with HAPI servers.

These definitions are provided first to ensure clarity in ensuing descriptions.

**parameter** – a measured science quantity or a related ancillary quantity; may be a scalar or may have one or  more dimensions; can have units and can have a fill value that represents no measurement or absent information

**dataset** – a collection with a conceptually uniform of set of parameters; a dataset is a logical entity, but it often physically resides in one or more files that are accessible online; a HAPI service presents a dataset as a seamless collection of records, offering a way to retrieve the ordered parameters while hiding actual storage details

**request parameter** – keywords in the keyword=value pairs that appear after the ‘?’ in a URL with a GET request. In this example GET request:

```
     http://example.com/data?id=alpha&time=2016-07-13 
```

the two request parameters, `id` and `time`, are shown in bold. To distinguish this from parameters in a dataset (first definition just above), these will always be called request parameters.

The HAPI specification consists of four endpoints that give clients a precise way to determine the data holdings of the server and to request data from the server.

1. describe the capabilities of the server; this lists the output formats the server can emit (CSV and binary)
2. list the datasets that are available; each dataset is associated with a unique id
3. obtain a description for dataset of a given id; the description defines the parameters in every dataset record
4. stream data content for a dataset of a given id; the streaming request must have time bounds (specified by request parameters time.min and time.max) and may indicate a subset of parameters (default is all parameters)

There is also a landing page endpoint that returns human-readable HTML, and although there is recommended content for this landing page, it is not essential to the functioning of the server.
The four endpoints are REST-like services, and the specification for each is described below.
 
## Endpoints
It is required that all endpoints are defined under a path with the name "hapi" to help distinguish services compliant with this specification from other services that may be available on a server.

The input specification for the endpoints is rigid so that for any endpoint that takes request parameters, only the parameters described below are allowed, and no extensions are permitted. If a HAPI server sees a request parameter that it does not recognize, it is required to throw an error indicating that the request is invalid (via HTTP 400 error – see below).  Ignoring unknown request parameters would falsely indicate to clients that the request parameter was understood and was taken into account when creating the output.

Because all requests to a HAPI server are for retrieving resources and will not change the server state, all HAPI endpoints must respond only to HTTP GET requests. Note that POST requests should result in an error. This represent a RESTful approach in which GET requests are restricted to be read-only operations from the server. The HAPI specification does not allow any input to the server (which for RESTful services are often implemented using POST requests). 

Error reporting by the server should be done use standard HTTP status codes. In an error response, the HTTP header should include the code and the body of the error message should include relevant details about the error. It is up to clients then how to ingest and/or display this error information. Servers are encouraged but not required to limit their return codes to the following suggested set:

**HTTP Status Codes**


| Status Code      | Description               || ---------------- | ------------------------- |
| 200              | OK – the user request was fully met with no errors |
| 400              | Bad request – something was wrong with the request, such as an unknown request parameter (misspelled or incorrect capitalization perhaps?), an unknown data parameter, an invalid time range, unknown dataset id |
| 500              | Internal error – the server had some internal fault or failure due to an internal software or hardware problem |


## HAPI Client Error Handling

Because servers are not required to limit HTTP return codes to those in the above table, clients should be able to handle the full range of HTTP responses. Also, a HAPI server may not be the only software to interact with a URL-based request from a HAPI server (there may be a load balancer or upstream request routing or caching mechanism in place), and thus it is just good client-side practice to be able to handle any HTTP errors.
The following pages give the detailed specification for the four main HAPI endpoints.


## hapi

This root endpoint serves as a human-readable landing page for the server. Unlike the other endpoints, there is no strict definition for the output, but It should include a brief description of the other endpoints, and links to documentation on how to use the server. An example landing page that can be easily customized for a new server is available at spase-group.org.
Sample Invocation

```
	http://example.com/hapi
```

**Request Parameters**

None

**Response**

The response is in HTML format and is not strictly defined, but should look something like the example below.

**Example**

Retrieve a listing of resources shared by this server.

```
http://example.com/hapi
```

**Response:**
```
<html>
<head> </head>
<body>
<h2> HAPI Server<h2>
<p> This server supports the HAPI 1.0 specification for delivery of time series
    data. The server consists of the following 4 RESST-like endpoints that will
    respond to HTTP GET requests.
</p>
<ol>
<li><a href="capabilities">capabilities</a> describe the capabilities of the server; this lists the output formats the server can emit (CSV and binary)</li>
<li><a href="catalog">catalog</a> list the datasets that are available; each dataset is associated with a unique id</li>
<li><a href="info">info</a> obtain a description for dataset of a given id; the description defines the parameters in every dataset record</li>
<li><a href="data">data</a> stream data content for a dataset of a given id; the streaming request must have time bounds (specified by request parameters time.min and time.max) and may indicate a subset of parameters (default is all parameters)</li>
<ol>
<p> For more information, see <a href="http://spase-group.org/hapi">this HAPI description</a> at the SPASE web site.  </p>
</body>
<html>
```

## capabilities

This endpoint describes relevant implementation capabilities for this server. Currently, the only possible variability from server to server is the list of output formats that are supported. The server is required to support the CSV format, but the binary format is optional.

**Sample Invocation**

```
	http://example.com/hapi/capabilities
```

**Request Parameters**

None

** Response**

Response is in JSON format [3] as defined by RFC-7159 and has a mime type of “application/json”. The format of the JSON response is defined in the section “HAPI Metadata.” The capabilities object has various keyword value pairs indicating the implemented features for a given server capability. Only one capability is currently in the spec, the “formats” capability, and it must be present in the response.

**Example**

Retrieve a listing of resources shared by this server.
```
http://example.com/hapi/capabilities
       Response:
{
   "HAPI" : "1.0",
   "capabilities" : 
   [
      {"formats": "csv”, “binary” ]}
   ]
}
```

If a server only reports an output format of CSV, then requesting data in binary form should cause the server to issue an HTTP return code of 400 (bad request).

##catalog

Provides a list of datasets available via this server.

**Sample Invocation**
```
	http://example.com/hapi/catalog
```

**Request Parameters**

None

**Response**

The response is in JSON format [3] as defined by RFC-7159 and has a mime type of “application/json”. An example is given here, and the content of the JSON response is fully defined in the section "HAPI JSON Content." The catalog is a simple listing of identifiers for the datasets available through the server providing the catalog. Additional metadata about each dataset is available through the info endpoint (described below). The catalog takes no query parameters and always lists the full catalog.

**Example**

Retrieve a listing of resources shared by this server.
```
http://example.com/hapi/catalog
       Response:
{
   "HAPI" : "1.0",
   "catalog" : 
   [
      {"id": "path/to/ACE_MAG"},
      {"id": "data/IBEX/ENA/AVG5MIN"},
      {"id": "data/CRUISE/PLS"},
      {"id": "any_identifier_here"}
   ]
}
```

Dataset identifiers in the catalog should be stable over time. Including version numbers or other revolving elements (dates, processing ids, etc.) in the datasets identifiers is not desirable. The intent of the HAPI specification is to allow data to be referenced using RESTful URLs that have a reasonable lifetime.

Also, note that the identifiers can have slashes in them. The identifiers should all be unique within a single HAPI server.

## info

Lists a data header for a given dataset, including a description of the parameters in the dataset.

**Sample Invocation**
```
	http://example.com/hapi/info?id=ACE_MAG
```

**Request Parameters**

| Name  | Description   || ----- | ------------- ||  id   | **Required**<br/> The identifier for the resource. || parameters | **Optional**<br/>A subset of the parameters to include in the header. |
**Response**

The response is in JSON format and provides metadata about one dataset. The focus is a list of parameters in the dataset, although there are several required and many optional descriptive keywords that can be included.  A short example is provided here, but the section below about “HAPI JSON Content” has many more details about possible header fields. Custom, user-defined keywords may also be present, but they must conform to the name specifications described in the section about.

**Example**
```
http://example.com/hapi/info?id=path/to/ACE_MAG
       Response:
{  "HAPI": "1.0",
   "createdAt”: "2016-06-15T12:34"
   "parameters": [
       { "name": "Time",
         "type": "isotime",
         "length": 24
       },
       { "name": "radial_position",
         "type": "double",
         "units": "km",
         "description": "radial position of the spacecraft" },
       { "name": "quality flag",
         "type": "integer",
         "units ": "none ",
         "description ": "0=OK and 1=bad " },
       { "name": "mag_GSE",
         "type": "float",
         "units": "nT",
         "size" : [3],
         "description": "hourly average Cartesian magnetic field in nT in GSE" }
   ]
}
```

The data header provided by the info endpoint is meant to provide all the details necessary for reader software to properly interpret the data stream for the associated dataset as returned by the data endpoint (described next). Of course the reader will also need to understand the layout of the data stream itself, and this is described below in the section “Streaming Formats.” The data endpoint can prefix the data with the same header information available from the info endpoint, but the info endpoint provides a way to learn about the content of dataset without requesting any data. Also, the data endpoint can return a data stream with no header, in which case the client would need to first have used the info endpoint to determine the structure, and then could make multiple requests for data streams without having to process a header for each one.

Both the info and data endpoints take an optional request parameter (recall the definition of request parameter in the introduction) called ‘parameters’ that allows users to restrict the dataset parameters listed in the data header and data stream, respectively. This enables clients (that already have a list of dataset parameters from a previous info or data request) to request a header for a subset of parameters that will match the data stream for the same subset of parameters.


## data

Provides access to a data resource and allows for selecting time ranges and fields to return. Data is returned in CSV format [2]. In each data record, all fields must have either a data value or a fill value of the same type as the quantities. The response may optionally contain a header. The header consists of the same JAON metadata available through the info endpoint, but with each header line prefixed with a hash (#) character. This simplifies the client’s job of isolating the header from the data.  Note that when a header is included, the data stream is not strictly in CSV format.

**Request Parameters**

Name
**Description**
| Name       | Description   || ---------- | ------------- || id         | **Required**<br/> The identifier for the resource. || time.min   | **Required**<br/> The smallest value of time to include in the response. || time.max   | **Required**<br/> The largest value of time to include in the response.  || parameters | **Optional**<br/> A comma separated list of parameters to include in the response Default is all parameters. || include    | **Optional**<br/> Has one possible value of “header” to indicate that the info header should precede the data. The header lines will be prefixed with the “#” character. || format     | **Optional**<br/> The desired format for the data stream. Possible values are "csv" and "binary". |
**Response**

Response is either in CSV format as defined by RFC-4180 and has a mime type of "text/csv" or in binary format where floating points number are in IEEE 754[5] format and byte order is LSB. Default data format is CSV.

If the header is requested, then each line of the header must begin with a hash (#) character. Otherwise, the contents of the header should be the same as returned from the info endpoint. When a data stream has an attached header an additional "format" attribute must be included in the header. The "format" attribute will define what format ("csv" or "binary") that the data is in. 

If a subset of parameters is requested, the time columns is always provided, even if it is not requested.

Two examples of data requests and responses are given – one with the header and one without.

 
**Examples**

Note that in this request, the header is requested, so the same header from the info request will be prepended to the data, but with a ‘#’ character as a prefix for every header line.

```
http://example.com/hapi/data?id=path/to/ACE_MAG&time.min=2016-01-01&time.max=2016-02-01&include=header
```

**Response:**
```
#{
#  "HAPI": "1.0",
#   "createdAt”: "2016-06-15T12:34"
#   "format": "csv",
#   "parameters": [
#       { "name": "Time",
#         "type": "isotime",
#         "length": 24
#       },
#       { "name": "radial_position",
#         "type": "double",
#         "units": "km",
#         "description": "radial position of the spacecraft"
#       },
#       { "name": "quality flag",
#         "type": "integer",
#         "units ": "none ",
#         "description ": "0=OK and 1=bad " 
#       },
#       { "name": "mag_GSE",
#         "type": "float",
#         "units": "nT",
#         "size" : [3],
#         "description": "hourly average Cartesian magnetic field in nT in GSE"
#       }
#   ]
#}
Time,radial_position,quality_flag,mag_GSE_0,mag_GSE_1,mag_GSE_2
2016-01-01T00:00:00.000,6.848351,0,0.05,0.08,-50.98
2016-01-01T01:00:00.000,6.890149,0,0.04,0.07,-45.26
		?
		? 
2016-01-01T02:00:00.000,8.142253,0,2.74,0.17,-28.62
```

The following example is the same, except it lacks the request to include the header.
```
http://example.com/hapi/data?id=path/to/ACE_MAG&time.min=2016-01-01&time.max=2016-02-01
```

**Response:**

If the data resource contains a time field, plus 3 data fields the response will be something like:

```
Time,radial_position,quality_flag,mag_GSE_0,mag_GSE_1,mag_GSE_2
2016-01-01T00:00:00.000,6.848351,0,0.05,0.08,-50.98
2016-01-01T01:00:00.000,6.890149,0,0.04,0.07,-45.26
		?
		? 
2016-01-01T02:00:00.000,8.142253,0,2.74,0.17,-28.62
```

## Representation of Time

The time format for the request must be a valid time string according to the ISO 8601 standard. Note that servers should be able to parse both the year-month-day (yyyy-mm-ddThh\:mm\:ss.sss) or day-of-year (yyyy-dddThh\:mm\:ss.sss) orientations of the time string. 
Also, in the data values returned, servers are allowed to use either form for ISO 8601 time strings, so clients must be able to transparently handle both flavors. A server should use a consistent time format for a single dataset.
Time values are considered to be relative to GMT. Time zones or time offsets are not allowed.  The trailing “Z” on the time string is allowed, and if not present, the time value is still assumed to be in GMT.
The time value of "0001-01-01T00\:00\:00.000" is considered a flag for an unknown or unspecified time value.
Servers should strictly obey the time bounds requested by clients. No data values outside the requested time range should be returned.


HAPI JSON Content
The capabilities , catalog, and info endpoints return JSON entities. (And the data endpoint may start with the JSON entity from the info endpoint prefixed by ‘#’ characters). The following tables describe the content specification for each of these JSON entities. Within the JSON response to an info request, there is a list of parameter objects, and the parameter specification is sufficiently complex to warrant its own table.
Additional Keyword / Value Pairs Allowed
While the HAPI server strictly checks all request parameters (servers must return an error code given any unrecognized request parameter as described earlier), the JSON content output by a HAPI server may contain additional, user-defined metadata elements.  All non-standard metadata keywords must begin with the prefix “x_” to indicate to HAPI clients that these are extensions. Custom clients could make use of the additional keywords, but standard clients would ignore the extensions. By using the standard prefix, the custom keywords will never conflict with any future keywords added to the HAPI standard. 

## Capabilities

| Node Name | Type   | Description || --------- | ------ | ----------- || HAPI      | string | **Required**<br/> The version number of the HAPI specification this description complies with. || capabilities | array(endpoint) |**Required**<br/> A list of capabilities offered by his server. |
**Endpoint Object**

| Node Name | Type   | Description || --------- | ------ | ----------- || formats   | string array | **Required**<br/> The list of output formats the serve can emit. The allowed values in the last are “csv” and “binary”. All HAPI servers must support at least “csv” output format, but “binary” output format is optional. |

##Catalog

| Node Name | Type   | Description || --------- | ------ | ----------- || HAPI      | string | **Required**<br/> The version number of the HAPI specification this description complies with. || catalog   | array(endpoint) | **Required**<br/> A list of endpoints available from this server. |
**Endpoint Object**

| Node Name | Type   | Description || --------- | ------ | ----------- || id        | string | **Required**<br/> The identifier that the host system uses to locate the resource. If the "id" is a URL it should be considered an reference to a HAPI service on another server. |
The schema of HAPI resource metadata is:

## Dataset Info

| Node Name   | Type   | Description || ----------- | ------ | ----------- || HAPI        | string | **Required**<br/> The version number of the HAPI specification this description complies with. || format      | string | **Required**<br/> when header precedes data.The format of the data string. Possible values of "csv" and "binary".|| parameters  | array(Parameter) | **Required**<br/> Description of the parameters in the data. || firstDate   | string | **Optional**<br/> ISO 8601 Date and Time of first record of data.|| lastDate    | string | **Optional**<br/> ISO 8601 Date and Time for the last record of data.|| time        | string | **Required**<br/> The name of time parameter.|| description | string | **Optional**<br/> A brief description of the resource.|| resourceURL | string | **Optional**<br/> The URL for the system to provide more detailed information about this data. || resourceID  | string | **Optional**<br/> The identifier assigned to the resource which can be used to obtain more information about this data. For example, the SPASE ID.|| creationDate | string | **Optional**<br/> ISO 8601 Date and Time of the list creation.|| modificationDate | string | **Optional**<br/> ISO 8601 Date and Time of the last modification of the resource.|| deltaTime    | string | **Optional**<br/> The ISO 8601 duration for the time difference between records.|| contact      | string | **Optional**<br/> Relevant contact person and possibly contact information.|| contactID    | string | **Optional**<br/> The identifier in the discovery system for information about the contact. For example, the SPASE ID of the person. |
## Parameter
 
| Node Name   | Type   | Description || ----------- | ------ | ----------- || name        | string | **Required**<br/>A short textual tag. || type        | string | **Required**<br/> One of "string", "double", "integer", “isotime". The "length" attributes determines precision. Default length for "double" is 8 bytes, "integer" is 4, others are 1 || length      | integer | **Required** for type ‘string’ and ‘isotime’; not allowed for others.<br/> The number of bytes or characters that contain the value. Valid only if data is streamed in binary format. || units       | string | **Required.**<br/> The units for the data values represented by this parameter. || size        | array(Integer) | **Required** for array parameters; not allowed for others.<br/> The number of elements in the array. Arrays are unwound and each column in the output is one slice of the array. Column names are the variable name with a numeric postfix, _0, _1, _2, etc. Clients reconstitute this array from the stream. | | fill        | string | **Optional**<br/> The value that indicates no valid data is present.|| description | string | **Optional**<br/> A brief description of the parameter.|| bins        | object | **Optional**<br/> For array parameters, the bins object describes the values associated with each element in the array. It the parameter represents a frequency spectrum, the bins object captures the frequency values for each frequency bin. The center value for each bin is required and the min and max values are optional. IF min or max is present, the other is also required. The bins object has a optional “units” keyword (any string value is allowed) and a required “values” keyword that holds a list of objects containing the “min”, “center”, and “max” for each bin. Below is an example for a variable that holds a proton energy spectrum. | 
 
## Examples

Data with a time field named "timestamp" and three data fields named Bx, By, Bz.
```
{
	"HAPI": "1.0",
	"firstDate": "2016-01-01T00:00:00.000",
	"lastDate": "2016-01-031T24:00:00.000",
	"time": "timestamp",
	"parameters": [
		{"name" : "timestamp", "type": "isotime", "units": "none"},
		{"name" : "bx", "type": "double", "units": "nT"},
		{"name" : "by", "type": "double", "units": "nT"},
		{"name" : "bz", "type": "double", "units": "nT"}		
	}
}
```

This example shows a header for the same data (time and three magnetic field components), but with the three components grouped into a one-dimensional array of size 3.
```
{
	"HAPI": "1.0",
	"firstDate": "2016-01-01T00:00:00.000",
	"lastDate": "2016-01-031T24:00:00.000",
	"time": "timestamp",
	"parameters": [
    { "name" : "timestamp",
      "type": "isotime",
      "units": "none"
    },
    { "name" : "b_field",
      "type": "double",
      "units": "nT",
      "size": [3]
    }
  ]
}
```

Here is an example showing a proton energy spectrum. Note that the uncertainty of the values in the proton spectrum is a separate variable. There is currently no way in the HAPIS spec to explicitly link a variable to its uncertainties.
```
{"HAPI": "1.0",
 "CreatedAt": "2016-06-15T12:34",

 "parameters": [
   { "name": "Time",
     "type": "isotime",
     "length": 24
   },
   { "name": "qual_flag",
     "type": "int"
   },
   { "name": "maglat",
     "type": "float",
     "units": "degrees",
     "description": "magnetic latitude"
   },
   { "name": "MLT",
     "type": "string",
     "length": 5,
     "units": "hours:minutes",
     "description": "magnetic local time in HH:MM"
   },
   { "name": "proton_spectrum",
     "type": "float",
     "size": [3],
     "units": "particles/(sec ster cm^2 keV)"
     "bins": {
         "units": "keV",
         "values": [
             { "min": 10.1, "center": 15, "max": 20 },
             { "min": 20,   "center": 25, "max": 30.3 },
             { "min": 30.3, "center": 35, "max": 40 }
              ]
          },
   { "name": "proton_spectrum_uncerts",
     "type": "float",
     "size": [3],
     "units": "particles/(sec ster cm^2 keV)"
     "bins": {
         "units": "keV",
         "values": [
             { "min": 10.1, "center": 15, "max": 20 },
             { "min": 20,   "center": 25, "max": 30.3 },
             { "min": 30.3, "center": 35, "max": 40 }
          ]
   }

  ]
}
```

## Implementation Notes

When the server sees a request parameter that it does not recognize is 
should throw an error. So given this query
```
time.min=T1&time.max=T2&fields=mag_GSE&avg=5s
```
If the server does not know about the avg=5s parameter, it should throw 
an error and not silently ignore it. The response should also use the
appropriate HTTP error code. For security, purposes, HAPI servers:

1. carefully screen incoming parameters values
2. do not include any unknown query parameters in the error response


## References

[1] ISO 8601:2004, http://dotat.at/tmp/ISO_8601-2004_E.pdf
[2] CSV format, https://tools.ietf.org/html/rfc4180
[3]  JSON Format, https://tools.ietf.org/html/rfc7159
[4] "JSON Schema", http://json-schema.org/
[5] EEE Computer Society (August 29, 2008). "IEEE Standard for Floating-Point Arithmetic". IEEE. doi:10.1109/IEEESTD.2008.4610935. ISBN 978-0-7381-5753-5. IEEE Std 754-2008

## Contact

The editor for this document is Todd King (tking@igpp.ucla.edu)


## Appendix A: JSON Schema[4] for HAPI Catalog Metadata
```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "title": "HAPI",
    "description": "A data resource description”,
    "type": "object",
    "required": ["HAPI", "firstDate", "lastDate", "time", "fields"]
    "properties": {
        "HAPI": {
            "description": "The version number of the HAPI specification this description is written for.",
            "type": "string"
        },
        "catalog": {
            "description": "A list of available resources.",
            "type": "array"
            "minItems": 1,
            "uniqueItems": true,
            "items": {
                "$ref": "#/endpoint"
            }
        }
	}
	"endpoint": {
	    "description": "Information about an available resource.",
		"type": "object",
		"properties": {
		   "id": "The identifier the host system uses to locate the resource.",
		   "type": "string"
		}
	}
}
```

## Appendix B: JSON Schema[4] for HAPI Resource Metadata
```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "title": "HAPI",
  "description": "A data resource description",
  "type": "object",
  "required": [
    "HAPI",
    "parameters"
  ],
  "properties": {
    "HAPI": {
      "description": "The version number of the HAPI specification this description is written for.",
      "type": "string"
    },
    "firstDate": {
      "description": "ISO 8601 Date and Time of first record of data. ",
      "type": "string"
    },
    "lastDate": {
      "description": "ISO 8601 Date and Time for the last record of data. ",
      "type": "string"
    },
    "time": {
      "description": "The name of time field.",
      "type": "string"
    },
    "parameters": {
      "description": "Description of the fields in the data.",
      "type": "array",
      "minItems": 1,
      "uniqueItems": true,
      "items": {
        "$ref": "#/parameter"
      }
    },
    "resourceURL": {
      "description": "The URL for the system to provide more detailed information about this data.",
      "type": "string"
    },
    "resourceID": {
      "description": "The identifier assigned to the resource which can be used to obtain more information about this data. For example, the SPASE ID.",
      "type": "string"
    },
    "creationDate": {
      "description": "ISO 8601 Date and Time of the list creation.",
      "type": "string"
    },
    "modifyDate": {
      "description": "ISO 8601 Date and Time of the last modification of the list.",
      "type": "string"
    },
    "deltaTime": {
      "description": "The ISO 8601 duration for the time difference between records.",
      "type": "string"
    },
    "contact": {
      "description": "Relevant contact person and possibly contact information.",
      "type": "string"
    },
    "contactID": {
      "description": "The identifier in the discovery system for information about the contact. For example, the SPASE ID of the person.",
      "type": "string"
    }
  },
  "parameter": {
    "type": "object",
    "required": [
      "name",
      "type"
    ],
    "properties": {
      "name": {
        "description": "Name of the parameter",
        "type": "string"
      },
      "type": {
        "description": "One of 'string', 'double', 'integer', 'isotime'.",
        "type": "string"
      },
      "units": {
        "description": "The units for all items in the parameter.",
        "type": "string"
      },
      "length": {
        "description": "The number of bytes or characters that contain the value.",
        "type": "number"
      },
      "size": {
        "description": "The number of fields in the stream included in this parameter. ",
        "type": "array",
        "minItems": 1,
        "items": {
          "type": "number"
        }
      },
      "fill": {
        "description": "The value that indicates no valid data is present.",
        "type": "string"
      },
      "description": {
        "description": "A brief description of the parameter.",
        "type": "string"
      }
    }
  }
}
```