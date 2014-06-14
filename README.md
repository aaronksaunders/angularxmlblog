Working with XML and JSON Strings in Bikes Project

When working on sample Ionic app for up coming workshop I needed to manipulate APIs.

As we all know, JSON is the best for an API to return, but sometimes it just doesnt happen that way. When working with the Capital Bikes data, the only way to get the station update feed is to use the XML response.

To convert my data within my application i deciced to use the xml2json.js library for simple conversion.

When the library is included the application like this, everything works fine.
[see sample below]

But since we are all good Angular developers, we know that the XML conversion might be used in mutiple places so lets create a nice factory for converting the data. So we create a factory like this and everything should work as expected.
[see sample below]

But is doesnt, we get an nasty error that just doesnt seem to make any sense

`ReferenceError: xml_str2json is not defined`

W