HAPI Data Access Specification
==============================

The Heliophysics Application Programmer's Interface (HAPI) data access specification is a RESTful API and streaming format specification for delivering digital time series data.

The HAPI specification describes a minimum set of capabilities needed for a server to allow access to the time series data values within one or more data collections.

**Current stable release:** [Version 1.1.0](https://github.com/hapi-server/data-specification/releases/tag/v1.1.0)
([PDF](https://github.com/hapi-server/data-specification/blob/master/HAPI-data-access-spec-1.1.0.pdf))

Current [draft version](https://github.com/hapi-server/data-specification/blob/master/HAPI-data-access-spec.md)

View [All Releases](https://github.com/hapi-server/data-specification/releases)

Other Files
===========

The other files in this project include:

**[example\_hapi\_landing\_page.html](https://github.com/hapi-server/data-specification/blob/master/example_hapi_landing_page.html)** : An example HAPI landing page you can use in your implementation of a HAPI data access server.

**[hapi\_header\_schema.json](https://github.com/hapi-server/data-specification/blob/master/hapi_header_schema.json)**: Schema for the information metadata returned by a
HAPI data access server. Written in [json-schema](http://json-schema.org).

**[servers.txt](https://github.com/hapi-server/data-specification/blob/master/servers.txt)**: A list of known HAPI data access servers.

Example Clients
===============

[Autoplot](http://autoplot.org/) (<http://autoplot.org/>): An interactive browser for data on the web. To connect to a HAPI data access server select `File` -\> `Add Plot From` -\> `hapi…`

Example Servers
===============

[TSDS](http://tsds.org/) Server (<http://tsds.org/>): A full-featured open-source stand-alone data server that is HAPI 1.0 compliant (soon to be migrated to HAPI 1.1).

More
====

Look for other example implementations under: <https://github.com/hapi-server>
