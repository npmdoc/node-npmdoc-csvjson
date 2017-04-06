# api documentation for  [csvjson (v4.1.3)](https://github.com/pradeep-mishra/csvjson#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-csvjson.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-csvjson) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-csvjson.svg)](https://travis-ci.org/npmdoc/node-npmdoc-csvjson)
#### convert csv to json and json to csv

[![NPM](https://nodei.co/npm/csvjson.png?downloads=true)](https://www.npmjs.com/package/csvjson)

[![apidoc](https://npmdoc.github.io/node-npmdoc-csvjson/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-csvjson_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-csvjson/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-csvjson/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-csvjson/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Pradeep Mishra"
    },
    "bugs": {
        "url": "https://github.com/pradeep-mishra/csvjson/issues"
    },
    "dependencies": {},
    "description": "convert csv to json and json to csv",
    "devDependencies": {
        "chai": "^1.10.0",
        "mocha": "^2.1.0"
    },
    "directories": {},
    "dist": {
        "shasum": "7edf115e5405dce68da6801c31b1b4cedbe99af3",
        "tarball": "https://registry.npmjs.org/csvjson/-/csvjson-4.1.3.tgz"
    },
    "gitHead": "dec7eec97d640910317d5c6c8ef13008df6e3603",
    "homepage": "https://github.com/pradeep-mishra/csvjson#readme",
    "keywords": [
        "csv",
        "csvtojson",
        "csv2json",
        "jsoncsv",
        "jsontocsv",
        "json2csv",
        "csv to json",
        "coloum-array",
        "schema json",
        "schema-json",
        "schema-csv",
        "json to csv"
    ],
    "license": "MIT",
    "main": "./index.js",
    "maintainers": [
        {
            "name": "pradeepmishra",
            "email": "pradeep.mishra@raweng.com"
        }
    ],
    "name": "csvjson",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/pradeep-mishra/csvjson.git"
    },
    "scripts": {
        "test": "mocha"
    },
    "version": "4.1.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module csvjson](#apidoc.module.csvjson)
1.  [function <span class="apidocSignatureSpan">csvjson.</span>toArray (data, opts)](#apidoc.element.csvjson.toArray)
1.  [function <span class="apidocSignatureSpan">csvjson.</span>toCSV (data, opts)](#apidoc.element.csvjson.toCSV)
1.  [function <span class="apidocSignatureSpan">csvjson.</span>toColumnArray (data, opts)](#apidoc.element.csvjson.toColumnArray)
1.  [function <span class="apidocSignatureSpan">csvjson.</span>toObject (data, opts)](#apidoc.element.csvjson.toObject)
1.  [function <span class="apidocSignatureSpan">csvjson.</span>toSchemaObject (data, opts)](#apidoc.element.csvjson.toSchemaObject)



# <a name="apidoc.module.csvjson"></a>[module csvjson](#apidoc.module.csvjson)

#### <a name="apidoc.element.csvjson.toArray"></a>[function <span class="apidocSignatureSpan">csvjson.</span>toArray (data, opts)](#apidoc.element.csvjson.toArray)
- description and source-code
```javascript
function toArray(data, opts){

    opts = opts || { };

    var delimiter   = (opts.delimiter || ',');
    var quote       = _getQuote(opts.quote);
    var content     = data;

    if(typeof(content) !== "string"){
        throw new Error("Invalid input, input data should be a string");
    }

    content = content.split(/[\n\r]+/ig);
    var arrayData = [ ];
    content.forEach(function(item){
        if(item){
            item = quote ?
                _convertArray(item, delimiter, quote) :
                item.split(delimiter);

            item = item.map(function(cItem){
                return _trimQuote(cItem);
            });
            arrayData.push(item);
        }
    });
    return arrayData;
}
```
- example usage
```shell
...
var options = {
  delimiter : ',' // optional
  quote     : '"' // optional
};

// for multiple delimiter you can use regex pattern like this /[,|;]+/

csvjson.toArray(data, options);

/*
returns
[
    ["sr","name","age","gender"],
    ["1","rocky","33","male"],
    ["2","jacky","22","male"],
...
```

#### <a name="apidoc.element.csvjson.toCSV"></a>[function <span class="apidocSignatureSpan">csvjson.</span>toCSV (data, opts)](#apidoc.element.csvjson.toCSV)
- description and source-code
```javascript
function toCSV(data, opts){

    opts                = (opts || { });
    opts.delimiter      = (opts.delimiter || ',');
    opts.wrap           = (opts.wrap || '');
    opts.arrayDenote    = (opts.arrayDenote && String(opts.arrayDenote).trim() ? opts.arrayDenote : '[]');
    opts.objectDenote   = (opts.objectDenote && String(opts.objectDenote).trim() ? opts.objectDenote : '.');
    opts.detailedOutput = (typeof(opts.detailedOutput) !== "boolean" ? true : opts.detailedOutput);
    opts.headers        = String(opts.headers).toLowerCase();
    var csvJSON         = { };
    var csvData         = "";

    if(!opts.headers.match(/none|full|relative|key/)){
      opts.headers = 'full';
    }else{
      opts.headers = opts.headers.match(/none|full|relative|key/)[0];
    }

    if(opts.wrap === true){
        opts.wrap = '"';
    }

    if(typeof(data) === "string"){
        data = JSON.parse(data);
    }

    _toCsv(data, csvJSON, "", 0, opts);

    var headers = _getHeaders(opts.headers, csvJSON, opts);

    if(headers){
      if(opts.wrap){
        headers = headers.map(function(item){
          return opts.wrap + item + opts.wrap;
        });
      }
      csvData = headers.join(opts.delimiter);
    }

    var bigArrayLen = _getBigArrayLength(csvJSON);
    var keys        = Object.keys(csvJSON);
    var row         = [ ];

    var replaceNewLinePattern = /\n|\r/g;
    if(!opts.wrap){
        replaceNewLinePattern = new RegExp('\n|\r|' + opts.delimiter, 'g');
    }


    for(var i = 0; i < bigArrayLen; i++){
        row = [ ];
        for(var j = 0; j < keys.length; j++){
            if(csvJSON[keys[j]][i]){
                csvJSON[keys[j]][i] = csvJSON[keys[j]][i].replace(replaceNewLinePattern, '\t');
                if(opts.wrap){
                    csvJSON[keys[j]][i] = opts.wrap + csvJSON[keys[j]][i] + opts.wrap;
                }
                row[row.length] = csvJSON[keys[j]][i];
            }else{
                row[row.length] = "";
            }
        }
      csvData += '\n' + row.join(opts.delimiter);
    }
    return csvData;
}
```
- example usage
```shell
...
    wrap  = <String|Boolean> optional default value is false
    headers = <String> optional supported values are "full", "none", "relative", "key"
    objectDenote = <String> optional default value is "."
    arrayDenote = <String> optional default value is "[]"
*/


csvjson.toCSV(data, options);

/*
returns

book.person[].firstName,book.person[].lastName,book.person[].age,book.person[].address.streetAddress,book.person[].address.city,
book.person[].address.state,book.person[].address.postalCode,book.person[].hobbies[]
Jane,Doe,25,21 2nd Street,Las Vegas,NV,10021-3100,gaming;volleyball
Agatha,Doe,25,21 2nd Street,Las Vegas,NV,10021-3100,dancing;politics
...
```

#### <a name="apidoc.element.csvjson.toColumnArray"></a>[function <span class="apidocSignatureSpan">csvjson.</span>toColumnArray (data, opts)](#apidoc.element.csvjson.toColumnArray)
- description and source-code
```javascript
function toColumnArray(data, opts){

    opts = opts || { };

    var delimiter   = (opts.delimiter || ',');
    var quote       = _getQuote(opts.quote);
    var content     = data;
    var headers     = null;

    if(typeof(content) !== "string"){
        throw new Error("Invalid input, input data should be a string");
    }

    content         = content.split(/[\n\r]+/ig);

    if(typeof(opts.headers) === "string"){
        headers = opts.headers.split(/[\n\r]+/ig);
        headers = quote ?
                _convertArray(headers.shift(), delimiter, quote) :
                headers.shift().split(delimiter);
    }else{
        headers = quote ?
                _convertArray(content.shift(), delimiter, quote) :
                content.shift().split(delimiter);
    }


    var hashData    = { };

    headers.forEach(function(item){
        hashData[item] = [];
    });

    content.forEach(function(item){
        if(item){
            item = quote ?
                  _convertArray(item, delimiter, quote) :
                  item.split(delimiter);
            item.forEach(function(val, index){
                hashData[headers[index]].push(_trimQuote(val));
            });
        }
    });

    return hashData;
}
```
- example usage
```shell
...
/*
  for importing headers from different source you can use headers property in options
  var options = {
headers : "sr,name,age,gender"
  };
*/

csvjson.toColumnArray(data, options);

/*
returns

{
    sr: [ '1', '2', '3' ],
    name: [ 'rocky', 'jacky', 'suzy' ],
...
```

#### <a name="apidoc.element.csvjson.toObject"></a>[function <span class="apidocSignatureSpan">csvjson.</span>toObject (data, opts)](#apidoc.element.csvjson.toObject)
- description and source-code
```javascript
function toObject(data, opts){

    opts = opts || { };

    var delimiter   = (opts.delimiter || ',');
    var quote       = _getQuote(opts.quote);
    var content     = data;
    var headers     = null;

    if(typeof(content) !== "string"){
        throw new Error("Invalid input, input data should be a string");
    }

    content = content.split(/[\n\r]+/ig);

    if(typeof(opts.headers) === "string"){
        headers = opts.headers.split(/[\n\r]+/ig);
        headers = quote ?
                _convertArray(headers.shift(), delimiter, quote) :
                headers.shift().split(delimiter);
    }else{
        headers = quote ?
                _convertArray(content.shift(), delimiter, quote) :
                content.shift().split(delimiter);
    }

    var hashData = [ ];

    content.forEach(function(item){
        if(item){
          item = quote ?
                _convertArray(item, delimiter, quote) :
                item.split(delimiter);
            var hashItem = { };
            headers.forEach(function(headerItem, index){
                hashItem[headerItem] = _trimQuote(item[index]);
            });
            hashData.push(hashItem);
        }
    });
    return hashData;
}
```
- example usage
```shell
...
/*
  for importing headers from different source you can use headers property in options
  var options = {
headers : "sr,name,age,gender"
  };
*/

csvjson.toObject(data, options);

/*
returns

[
    {
        sr : 1,
...
```

#### <a name="apidoc.element.csvjson.toSchemaObject"></a>[function <span class="apidocSignatureSpan">csvjson.</span>toSchemaObject (data, opts)](#apidoc.element.csvjson.toSchemaObject)
- description and source-code
```javascript
function toSchemaObject(data, opts){

    opts = opts || { };

    var delimiter   = (opts.delimiter || ',');
    var quote       = _getQuote(opts.quote);
    var content     = data;
    var headers     = null;
    if(typeof(content) !== "string"){
        throw new Error("Invalid input, input should be a string");
    }

    content         = content.split(/[\n\r]+/ig);


    if(typeof(opts.headers) === "string"){
        headers = opts.headers.split(/[\n\r]+/ig);
        headers = quote ?
                _convertArray(headers.shift(), delimiter, quote) :
                headers.shift().split(delimiter);
    }else{
        headers = quote ?
                _convertArray(content.shift(), delimiter, quote) :
                content.shift().split(delimiter);
    }


    var hashData    = [ ];

    content.forEach(function(item){
        if(item){
          item = quote ?
                _convertArray(item, delimiter, quote) :
                item.split(delimiter);
            var schemaObject = {};
            item.forEach(function(val, index){
                _putDataInSchema(headers[index], val, schemaObject , delimiter, quote);
            });
            hashData.push(schemaObject);
        }
    });

    return hashData;
}
```
- example usage
```shell
...
/*
  for importing headers from different source you can use headers property in options
  var options = {
headers : "created,contact.name,contact.age+,contact.number+,address[],address[],contact.hobbies[;],-id,friends[0].name,friends[
0].phone,friends[1].name,friends[1].phone"
  };
*/

csvjson.toSchemaObject(data, options)

/*
returns

[
    {
        "created":"2014-11-12",
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
