---
title: "JSON Data Standard - IETF RFCs - IANA"
date: 2025-08-31T00:00:00+00:00
hero: json.png
description: JSON Data Standard - IETF RFCs - IANA
menu:
  dotnet:
    name: JSON Data Standard - IETF RFCs - IANA
    identifier: JSON-Data-Standard-IETF-RFCs-IANA
    weight: -20250831
tags: [JSON Data Standard, IETF RFCs, IANA]
categories: [JSON Data Standard, IETF RFCs, IANA]

author:
  name: Akif T.
---

<p class="d-flex justify-content-center">
<img src="json.png" alt="json" title="json" style="border-radius: 20px;"><br>
</p>


#### **JSON Data Standard - IETF RFCs - IANA**

```JavaScript Object Notation (JSON)``` is a lightweight data-interchange format. JSON is easy for humans to read and write. ```JSON``` is easy for machines to parse and generate. 

```JSON``` can represent;
- four ```primitive``` types (strings, numbers, booleans, and null)
- two ```structured``` types (objects and arrays).

<kbd>JSON Data Types - IETF RFCs</kbd>
```
String: A sequence of characters enclosed in double quotes.  
Number: A numerical value, which can be an integer or a floating-point.  
Boolean: A true or false value.  
Null: A null value, representing the absence of a value.  
Array: An ordered list of values, which can be of any data type.  
Object: A collection of key-value pairs, where keys are strings and values can be any data type.  
```

The ```set of tokens``` includes six structural characters, strings, numbers, and three literal names.  

<kbd>JSON Tokens - IETF RFCs</kbd>
```
begin-array     = [ left square bracket  
begin-object    = { left curly bracket  
end-array       = ] right square bracket  
end-object      = } right curly bracket  
name-separator  = : colon  
value-separator = , comma  
```

A JSON value MUST be an ```object```, ```array```, ```number```, or ```string```, or one of the following ```three literal names```:   
  
<kbd>JSON Literal Names - IETF RFCs</kbd>
``` 
- false  
- null  
- true  
```
  


##### **MIME Type for JSON - IETF RFCs - IANA**

A ```type``` and a ```subtype```, separated by a ```slash (/)``` with no whitespace between:  
```type/subtype```  

<kbd>JSON Tokens - IETF RFCs</kbd>
```
Type name:  application  
Subtype name:  json  
Required parameters:  n/a  
Optional parameters:  n/a  
Encoding considerations:  binary  
```

The media type for ```JSON``` text is ```application/json```.  

```The Internet Assigned Numbers Authority (IANA)``` is responsible for all official ```MIME types```, and you can find the most up-to-date and ```complete list```:  
https://www.iana.org/assignments/media-types/media-types.xhtml



#### **Examples**

<kbd>*.json</kbd>
```
{
  "string": "Hello, World!",
  "number": 1,
  "boolean": true,
  "nullValue": null,
  "array": [1, 2, 3, "four", false],
  "object": {
    "key1": "value1",
    "key2": 2,
    "key3": {
      "nestedKey": "nestedValue"
    }
  }
}
```

```"string": "Hello, World!"```  
This line defines a key named string with a value of "Hello, World!". The value is a string data type, enclosed in double quotes.  
  
```"number": 1```  
Here, the key number is associated with the integer value 1. This demonstrates the number data type in JSON.  
  
```"boolean": true```  
The key boolean is set to true, showcasing the boolean data type. It can also be set to false.  
  
```"nullValue": null```  
This line defines a key nullValue with a value of null, indicating the absence of a value.  
  
```"array": [1, 2, 3, "four", false]```  
The key array contains an array of mixed data types: integers (1, 2, 3), a string ("four"), and a boolean (false). The array is enclosed in square brackets [].  
  
```"object": { ... }```  
The key object contains another JSON object, which is defined within its own set of curly braces {}. This object has its own key-value pairs:  
"key1": "value1": A string key-value pair.  
"key2": 2: A number key-value pair.  
"key3": { "nestedKey": "nestedValue" }: This key contains another nested object, demonstrating how objects can be nested within other objects.  



#### **References**

[RFC8259] - The JavaScript Object Notation (JSON) Data Interchange Format  
https://datatracker.ietf.org/doc/html/rfc8259  

[RFC4627] - The application/json Media Type for JavaScript Object Notation (JSON)  
https://datatracker.ietf.org/doc/html/rfc4627  

[RFC6838] - Media Type Specifications and Registration Procedures  
https://datatracker.ietf.org/doc/html/rfc6838  

Working with JSON  
https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/JSON  

JSON Data (Standard)  
https://docs.oracle.com/en/database/oracle/oracle-database/21/adjsn/json-data.html  

IANA - Media Types  
https://www.iana.org/assignments/media-types/media-types.xhtml  
