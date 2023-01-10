# Changelog for HAPI 3.0.0

## v2 to v3 API Changes
Non-backward compatible changes to the request interface in HAPI 3.0:

* The URL parameter id was replaced with dataset.
* time.min and time.max were replaced with start and stop, respectively.
* Addition of a new endpoint, "about", for server description metadata.

These changes were discussed in issue #77. HAPI 3 servers must accept both the old and these new parameter names, but the HAPI 2 specification requires an error response if the new URL parameter names are used. In a future version, the deprecated older names will no longer be valid.

## v2 to v3 Schema Changes
* Ability to specify time-varying bins (#83)
* Ability to use JSON references in info response (#82)
* Ability to indicate a units schema (if one is being used for units strings) (#81)
* Added citation element

# Changelog for HAPI 2.1.1

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

# Changelog for HAPI version 2.1.0

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
