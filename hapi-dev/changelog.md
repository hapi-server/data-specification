Changelog for the HAPI Specification
==================

For each version, there may be up to three types of changes to the specification:
* API changes - new or different features to the request interface
* Response Format Changes - new or different items in the server responses
* Clarifications - better description or handling of corner cases, etc

Some API and Response format changes may be non-backward compatible, but this is so far only true in Version 3.0.

# Version 3.3

## API Changes

* add new request parameter option `resolve_references=false` to `catalog` and `info` endpoints to tell server to not perform reference substitution ([#220](https://github.com/hapi-server/data-specification/pull/220))

## Response Format Changes

* added optional location and geoLocation attributes to info response for indicating where measurement were made ([#238](https://github.com/hapi-server/data-specification/pull/238))
* added altitude quantity to list of valid vector component types, since this is common for geo-location values ([#233](https://github.com/hapi-server/data-specification/pull/233))
* now have distinct `serverCitation` in `about` endpoint and `datasetCitation` in `info` endpoint, and plain `citation` is deprecated ([#235](https://github.com/hapi-server/data-specification/pull/235))
* added keywords in several places so that HAPI can describe FAIR data and added section on how to FAIR principles map to HAPI ([#224](https://github.com/hapi-server/data-specification/pull/224))
* added optional `warning` and `note` attributes to `about` and `info` endpoints ([#223](https://github.com/hapi-server/data-specification/pull/223))

## Clarifications

* clarified how to name a custom (i.e., non-standard) endpoint ([#245](https://github.com/hapi-server/data-specification/pull/245))
* clarified how scalar parameters can also contain vector components ([#244](https://github.com/hapi-server/data-specification/pull/244))
* clarified requirements for which of bin centers and ranges need to be present ([#237](https://github.com/hapi-server/data-specification/pull/237))
* clarified that HAPI is RESTful rather than strictly based on the original REST concept ([#236](https://github.com/hapi-server/data-specification/pull/236))
* clarified that any non-standard data format in the `outputFormats` of the `capabilities` endpoint needs to begin with `x_` ([#222](https://github.com/hapi-server/data-specification/pull/222))
* clarified difference between `title` (a short label) in `catalog` endpoint versus the `description` (few lines of details) in `info` endpoint ([#221](https://github.com/hapi-server/data-specification/pull/221))
* clarified expectations for `id` and `title` in `about` endpoint (acronyms ok in id, but expand in title; don't include word HAPI) ([#219](https://github.com/hapi-server/data-specification/pull/219))
* fixed some typos and inconsistencies ([#241](https://github.com/hapi-server/data-specification/pull/241))
* rearranged `info` section for clarity ([#777](https://github.com/hapi-server/data-specification/pull/777))

# Version 3.2

## API Changes

Version 3.2 is backward compatible with 3.1.

There is a new optional way to query the `catalog` endpoint, which now takes a request parameter
called `depth` to indicate how much detail the catalog response should include. The catalog
can now include all the elements in each dataset's `info` response.
The `capabilities` endpoint advertises if this functionality is supported. ([#164](https://github.com/hapi-server/data-specification/pull/164))

The spec document now suggests a way for HAPI clients to identify themselves as bots or
non-human users to a HAPI server. Doing this can help server administrators / developers in logging actual
science usage, as opposed to web scraping or mirroring activity. This is not a change in the spec.
([#174](https://github.com/hapi-server/data-specification/pull/174))

## Response Format Changes

There is now an error code for when the `depth` request parameter to the `catalog` endpoint is invalid.
Only specific values are allowed for `depth.` ([#191](https://github.com/hapi-server/data-specification/pull/191))

The `capabilities` now includes a way to ping the server to test that it is functioning. The way to do
this is to make simple data request, and the optional new info the `capabilities` response allows a
server to identify exactly what data request to use for this kind pf ping.
([#172](https://github.com/hapi-server/data-specification/pull/172))

There have been many requests for HAPI to also serve images. To accomodate this, string parameters
now can be identified as being URIs which then point to image files. This also enables HAPI to
more uniformly offer lists of any kind of file. This file listing capability should be viewed as
a different kind of service from the usual numeric data serving capability offered by HAPI. 
See 3.6.16 for more details and also ([#166](https://github.com/hapi-server/data-specification/pull/166))

## Clarifications

The error message text was made more precise for HAPI error codes related to invalid start and stop times in a data request.
([#163](https://github.com/hapi-server/data-specification/pull/163))

When describing response formats offered by HAPI, it is now emphasized that the output formats offered by HAPI are
transport formats meant for streaming data and are not intended to be used as traditional file formats. 
([#159](https://github.com/hapi-server/data-specification/pull/159))

Clarified that a HAPI request with an empty string after `parameters=` is the same as
not requesting any specific paramters, which defaults to requesting all parameters.
([#201](https://github.com/hapi-server/data-specification/pull/201))

# Version 3.1

Version 3.1 is backward compatible with 3.0. It adds support for three optional aspects in the `info` response:

List of changes from 3.0.1 is [here](https://github.com/hapi-server/data-specification/compare/cfed14f74995b39598b43e1976be702f2c8350c4..964f44f8bbe07f5d3fd97fb8adb07ab71debb328)

## Response Format Changes

1. support for vector quantities: parameters that are vector quantities can optionally specify a coordinate system and can identify vector components as such; datasets can optionally specify a coordinate system schema ([#115](https://github.com/hapi-server/data-specification/issues/115))
1. a dataset may optionally include other types of metadata inside a separate block ([#117](https://github.com/hapi-server/data-specification/issues/117))
1. each dataset may optionally indicate a maximum time range to request data ([#136](https://github.com/hapi-server/data-specification/issues/136))


# Version 3.0.1

## Clarifications

Added statement that `dataset` and `parameters` may not contain Unicode but that this support will be added in 3.1. See [GitHub Issue #128](https://github.com/hapi-server/data-specification/issues/128).


# Version 3.0

## API Changes

Non-backward compatible changes to the request interface in HAPI 3.0:

* The URL parameter `id` was replaced with `dataset`.
* `time.min` and `time.max` were replaced with `start` and `stop`, respectively.
* Addition of a new endpoint, `about`, for server description metadata.

These changes were discussed in issue #77. HAPI 3 servers must accept both the old and these new parameter names, but the HAPI 2 specification requires an error response if the new URL parameter names are used. In a future version, the deprecated older names will no longer be valid.

## Response Format Changes
* Ability to specify time-varying bins (#83)
* Ability to use JSON references in info response (#82)
* Ability to indicate a units schema (if one is being used for units strings) (#81)


This URL generates a diff:

https://github.com/hapi-server/data-specification/compare/4702968b13439af684d43416b442c534bf569f6c..4a02df680b76d757a7cbf4a06e55c53b9b91e310

## Clarifications:

deprecated the use of all uppercase for the time stamp location options in favor of all lowercase, which is more consistent with the rest of the specification

replaced "hapi-server.org" with just "server" in URLs

clarified what the length attribute means for data parameters

clarified how to specify array sizes when one array dimension size is 1

changed the definition of 'units' to allow for different units in the elements of an array

changed the definition of 'label' to allow for different labels for each element in the array

now allow multi-dimensional data to not have bins in every dimension; any dimenions with null for 'centers' and nothing present for 'ranges' will not be considered to have any binning in that dimension

clarified that reordering parameters in a request has no effect on the order of the parameters in the data returned by the server (order of the fields returned is always the same regardless of the order in which they were requested)

clarified server responses when time range has no data (HTTP status of 200 and HAPI status of 1201 and return no data content) or data is all fill (HTTP status of 200 and HAPI status of 1200 and return the fill values)

more clarification and examples for error messages from HAPI servers

fixed multiple typos and made wording improvements



# Version 2.1.1

These are all small clarifications on relatively infrequent situations.

Pull request for going from 2.1.0 to 2.1.1:
https://github.com/hapi-server/data-specification/pull/93

This URL shows differences between 2.1.0 and 2.1.1:
https://github.com/hapi-server/data-specification/compare/b85e1db..8969633

## Clarifications

* updated version number to 2.1 when used in text and in example output
* clarified how to indicate a dimensionless quantity within an array of units for an array-valued parameter with non-uniform units; see Issue #85
* clarified the use of scalar and array values for labels and units that describe an array-valued parameter; see Issue #91
* clarified that `null` is not allowed as a value within a `centers` or `ranges` array in a `bins` description; see Issue #86

In a future release, there will be an occasion to use `null` values for some bin definitions, but only when the bin `centers` and `ranges` are able to be specified as time-varying elements within the data (as opposed to fixed quantities in the `info` metadata). This is expected to be included in verion 3.0.

# Version 2.1.0

## Clarifications 

1. replaced "hapi-server.org" with just "server" in example URLs
2. clarified what the length attribute means for data parameters
3. clarified how to specify array sizes when one array dimension size is 1
4. more clarification and examples for error messages from HAPI servers
5. fixed multiple typos and made wording improvements

## Response Format Changes

1. changed the definition of 'units' to allow for different units in the elements of an array
2. changed the definition of 'label' to allow for different labels for each element in the array
3. deprecated the use of all uppercase for the time stamp location options in favor of all lowercase, which is more consistent with the rest of the specification
4. now allow multi-dimensional data to not have bins in every dimension; any dimensions with null for 'centers' and nothing present for 'ranges' will not be considered to have any binning in that dimension

## API Changes

1. add HAPI 1411 error `out of order or duplicate parameters`
2. clarified server responses when the time range has no data (HTTP status of 200 and HAPI status of 1201 and return no data content) or data is all fill (HTTP status of 200 and HAPI status of 1200 and return the fill values)


# Version 2.0

Difference between 1.1 and 2.0: https://www.diffchecker.com/sBWweoDa

Summary of changes:
1. time values output by the server now must end with "Z" (not backward compatible) [diff](https://github.com/hapi-server/data-specification/commit/9bcb2f43014a05380425c8e2be24b457da7c5542)
2. the "units" on the bins are required (not backward compatible) [diff](https://github.com/hapi-server/data-specification/commit/30d8c967e252069a256019b71537694cd7fe7f97)
3. incoming time values (in request URL) should have a trailing "Z" to indicate GMT+0, but are interpreted as such even if there is no trailing Z [diff](https://github.com/hapi-server/data-specification/commit/a7a528b455b57a02987fb50b5e8c4890b721e774)
4. the length value for strings is now consistently described; all unused characters in the string should be filled with null characters (so no null terminator is needed if the string is exactly the given length)
[diff 1](https://github.com/hapi-server/data-specification/commit/4d8395578c42a7a20fd8cd0895c6cedb5b28abc7) [diff 2](https://github.com/hapi-server/data-specification/commit/32f2b219fdd6e8f7c33546077f9dc95f908b0752) [diff 3](https://github.com/hapi-server/data-specification/commit/c0aa8b9ca2f3c0d573a40019bdaf5c6a1edb1008) [diff4](https://github.com/hapi-server/data-specification/commit/533120bcd3622b58ae0f776f2e19fd854b4d3b54)
5. in CSV output, the use of quotes for string values with commas is clarified [diff](https://github.com/hapi-server/data-specification/commit/a1c444298f4cb0c7da2dfeb25b40879a80d100b0)
6. the subset of ISO8601 that we use is better described [diff](https://github.com/hapi-server/data-specification/commit/a7a528b455b57a02987fb50b5e8c4890b721e774)
7. added `timeStampLocation` [diff](https://github.com/hapi-server/data-specification/commit/9ee24e0e3f9c09243b0762664e13adbf446024b8) [diff](https://github.com/hapi-server/data-specification/commit/2603550251fefee14d4f2192095981c35753a2f3)

# Version 1.1

# Version 1.0

Initial version!
