# Changelog for HAPI specification version 2.1.0

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
