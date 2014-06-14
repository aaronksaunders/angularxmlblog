Working with XML and JSON Strings in Bikes Project
====
When working on sample Ionic Framework app for up coming workshop I needed to manipulate APIs.

As we all know, JSON is the best for an API to return, but sometimes it just doesnt happen that way. When working with the [Capital Bikeshare data](https://www.capitalbikeshare.com/system-data), the only way to get the station update feed is to use the XML response.

To convert my data within my application i deciced to use the [xml2json.js](https://www.npmjs.org/package/xml2json) library for simple conversion.

When the library is included the application like this, everything works fine.
```JavaScript
var x2js = new X2JS();
var jsonObject = xmlParser.xml_str2json(string);
console.log(JSON.stringify(jsonObject));
```

But since we are all good Angular developers, we know that the XML conversion might be used in mutiple places so lets create a nice factory for converting the data. So we create a factory like this and everything should work as expected.

```JavaScript
angular.module('myApp', [])
.factory('xmlParser', function () {
  var x2js = new X2JS();
  return {
    xml2json: x2js.xml2json,
    xml_str2json : x2js.xml_str2json,
    json2xml: x2js.json2xml_str
  }
})
```
But is doesnt, we get an nasty error that just doesnt seem to make any sense

`TypeError: undefined is not a function`

As it turns out when the factory is used and the function `x2js.xml_str2json` is called, it is not bound to the correct context so the function is failing. A little bit of a deeper dive into the x2js code revealed that I would need to bind the context of the object when making the call to get the correct results.

Use of the [`angular.bind`](https://docs.angularjs.org/api/ng/function/angular.bind) function was the missing link, see code below.

```Javascript
angular.module('myApp', [])
.factory('xmlParser', function () {
  var x2js = new X2JS();
  return {
    xml2json: x2js.xml2json,
    xml_str2json: function (args) {
      return angular.bind(x2js, x2js.xml_str2json, args)();
    },
    json2xml: x2js.json2xml_str
  }
})
```
