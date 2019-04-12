Changelog for HAPI version 2.1.0

This URL generates a diff:

https://github.com/hapi-server/data-specification/compare/4702968b13439af684d43416b442c534bf569f6c..4a02df680b76d757a7cbf4a06e55c53b9b91e310

Changes:

deprecated the use of all uppercase for the time stamp location options in favor of all lowercase, which is more consistent with the rest of the specification

replaced "hapi-server.org" with just "server" in URLs

clarified what the length attribute means for data parameters

clarified how to specify array sizes when one array dimension size is 1

changed the definition of 'units' to allow for different units in the elements of an array

changed the definition of 'label' to allow for different labels for each element in the array

now allow multi-dimensional data to not have bins in every dimension by have that dimenions have null for 'centers' nad 'ranges' be absent

clarified that reordering parameters in a request has no effect on the order of the parameters in the data returned by the server

clarified server responses when time range has no data (HTTP status of 200 and HAPI status of 1201 and return no data content) or data is all fill (HTTP status of 200 and HAPI status of 1200 and return the fill values)

more clarification and examples for error messages from HAPI servers

fixed multiple typos and made wording improvements
