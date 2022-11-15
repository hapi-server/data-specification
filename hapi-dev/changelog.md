Changelog for the HAPI Specification
==================

For each version, there may be up to three types of changes to the spceification:
* API changes - new or different features to the request interface
* Response Format Changes - new or different items in the server responses
* Clarifications - better description or handling of corner cases, etc

Some API and Response format changes may be non-backwards compatible, but this is so faronly true at Version 3.0.

# Version 3.1

# Version 3.0.1

Added statement that `dataset` and `parameters` may not contain Unicode but that this support will be added in 3.1. See [GitHub Issue #128](https://github.com/hapi-server/data-specification/issues/128).


# Version 3.0

## API Changes

Non-backward compatible changes to the request interface in HAPI 3.0:

* The URL parameter id was replaced with dataset.
* time.min and time.max were replaced with start and stop, respectively.
* Addition of a new endpoint, "about", for server description metadata.

These changes were discussed in issue #77. HAPI 3 servers must accept both the old and these new parameter names, but the HAPI 2 specification requires an error response if the new URL parameter names are used. In a future version, the deprecated older names will no longer be valid.

## Response Changes
* Ability to specify time-varying bins (#83)
* Ability to use JSON references in info response (#82)
* Ability to indicate a units schema (if one is being used for units strings) (#81)


(Text below was from a section labeld **Changes from 2.1.0 to 3.0.0**) so the items in this list that are from 2.1.0 to 2.1.1 should be removed.

This URL generates a diff:

https://github.com/hapi-server/data-specification/compare/4702968b13439af684d43416b442c534bf569f6c..4a02df680b76d757a7cbf4a06e55c53b9b91e310

Changes:

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

* updated version number to 2.1 when used in text and in example output
* clarified how to indicate a dimensionless quantity within an array of units for an array-valued parameter with non-uniform units; see Issue #85
* clarified the use of scalar and array values for labels and units that describe an array-valued parameter; see Issue #91
* clarified that `null` is not allowed as a value within a `centers` or `ranges` array in a `bins` description; see Issue #86

In a future release, there will be occasion to use `null` values for some bin definitions, but only when the bin `centers` and `ranges` are able to be specificed as time varying elements within the data (as opposed to fixed quantities in the `info` metadata). This is expected to be included in verion 3.0.

# Version 2.1.0

## Documentation

1. replaced "hapi-server.org" with just "server" in URLs
2. clarified what the length attribute means for data parameters
3. clarified how to specify array sizes when one array dimension size is 1
4. more clarification and examples for error messages from HAPI servers
5. fixed multiple typos and made wording improvements

## Schema

1. changed the definition of 'units' to allow for different units in the elements of an array
2. changed the definition of 'label' to allow for different labels for each element in the array
3. deprecated the use of all uppercase for the time stamp location options in favor of all lowercase, which is more consistent with the rest of the specification
4. now allow multi-dimensional data to not have bins in every dimension; any dimenions with null for 'centers' and nothing present for 'ranges' will not be considered to have any binning in that dimension

## Server API

1. add HAPI 1411 error `out of order or duplicate parameters`
2. clarified server responses when time range has no data (HTTP status of 200 and HAPI status of 1201 and return no data content) or data is all fill (HTTP status of 200 and HAPI status of 1200 and return the fill values)


# Version 2.0

Difference between 1.1 and 2.0: https://www.diffchecker.com/sBWweoDa

Summary of changes:
1. time values output by the server now must end with "Z" (not backwards compatable) [diff](https://github.com/hapi-server/data-specification/commit/9bcb2f43014a05380425c8e2be24b457da7c5542)
2. the "units" on the bins are required (not backwards compatable) [diff](https://github.com/hapi-server/data-specification/commit/30d8c967e252069a256019b71537694cd7fe7f97)
3. incoming time values (in request URL) should have a trailing "Z" to indicate GMT+0, but are interpreted as such even if there is no trailing Z [diff](https://github.com/hapi-server/data-specification/commit/a7a528b455b57a02987fb50b5e8c4890b721e774)
4. the length value for strings is now consistently described; all unused characters in the string should be filled with null characters (so no null terminator is needed if the string is exactly the given length)
[diff 1](https://github.com/hapi-server/data-specification/commit/4d8395578c42a7a20fd8cd0895c6cedb5b28abc7) [diff 2](https://github.com/hapi-server/data-specification/commit/32f2b219fdd6e8f7c33546077f9dc95f908b0752) [diff 3](https://github.com/hapi-server/data-specification/commit/c0aa8b9ca2f3c0d573a40019bdaf5c6a1edb1008) [diff4](https://github.com/hapi-server/data-specification/commit/533120bcd3622b58ae0f776f2e19fd854b4d3b54)
5. in CSV output, the use of quotes for string values with commas is clarified [diff](https://github.com/hapi-server/data-specification/commit/a1c444298f4cb0c7da2dfeb25b40879a80d100b0)
6. the subset of ISO8601 that we use is better described [diff](https://github.com/hapi-server/data-specification/commit/a7a528b455b57a02987fb50b5e8c4890b721e774)
7. added `timeStampLocation` [diff](https://github.com/hapi-server/data-specification/commit/9ee24e0e3f9c09243b0762664e13adbf446024b8) [diff](https://github.com/hapi-server/data-specification/commit/2603550251fefee14d4f2192095981c35753a2f3)

# Version 1.1


# Version 1.0

Initial version!













