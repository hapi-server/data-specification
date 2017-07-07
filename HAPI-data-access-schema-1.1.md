The file `HAPI-data-access-schema-1.1.json` contains several JSON schema for HAPI server responses.

To use it, one must resolve the references.  For example, using `nodejs` and the `jsonschema` package,

```javascript
var fs = require('fs');
var schema = fs.readFileSync("HAPI-data-access-schema-1.1.json");
var schema = JSON.parse(schema);

// Capabilities response from server
var json = {"outputFormats":["csv","binary"],"HAPI":"1.1","status":{"code":1200,"message":"OK"}};

var Validator = require('jsonschema').Validator;
var v = new Validator();
v.addSchema(schema, '/HAPI');
v.addSchema(schema, '/HAPIDateTime');
v.addSchema(schema, '/HAPIStatus');
vr = v.validate(json, schema['capabilities']);
console.log(vr)
```