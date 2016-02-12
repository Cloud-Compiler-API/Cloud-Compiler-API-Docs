---
title: Documentation | Cloud Compiler API
  
toc_footers:
  - <p>&nbsp;&nbsp;&nbsp;&nbsp;Developed by <a href="https://github.com/harish095">Harish Kandala</a></p>

includes:
  - errors

search: true
---

# Introduction

**Cloud Compiler API** is the only free API which compiles the source code provided by the user in the cloud and returns the output. Currently this API supports **70+** programming languages.

No authorization key is required to access this API. Anyone can directly access the API at the endpoints as given in the documentation.

Go through the documentation given below to get familiar with the API. If you have any issues or trouble using this API, please report it to us.

All URLs referenced in the documentation have the following base:

<div id="mainlogo" style="position: absolute; width: 50%; right: 0; top: 250px;">
    <img src="images/mainlogo.png" style="display: block; margin: 0 auto; width: 60%; max-width: 380px;"/>
</div>

<aside class="notice" style="text-align: center; font-size: 15px; font-weight: 600; margin-bottom: 0;">
  http://cloudcompiler.esy.es/api/
</aside>


## Functionality

Cloud Compiler API allows you to do the following actions:

* Upload a source code;

* Run the program with input data on server side in more than 70 programming languages;

* And download results of the execution (output, standard error, compilation information, execution time, memory usage, etc.).

## Web Service

All API URLs listed in this documentation are relative to **http://cloudcompiler.esy.es/api/**. For example, the <a href="#get-all-supported-languages">/languages</a> API call is reachable at **http://cloudcompiler.esy.es/api/languages**

The Cloud Compiler API is a mostly RESTful API. Known caveats:

* All API calls should be made with HTTP GET or POST.

* You can consider any non-200 HTTP response code as an error - the returned data will contain more detailed information

* Note that Cloud Compiler error codes mentioned below are not same as http error codes.

# Methods

## Get all supported languages

```shell
curl "http://cloudcompiler.esy.es/api/languages?onlyPopular=1"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 5,
    "name": "Bash"
  },
  {
    "id": 8,
    "name": "C"
  },
  {
    "id": 10,
    "name": "C#"
  },
  {
    "id": 13,
    "name": "C++ 5.1"
  },
  { 
    "id": 14,
    "name": "C++14"
  },
  {
    "id": 35,
    "name": "Haskell"
  },
  {
    "id": 38,
    "name": "Java"
  },
  {
    "id": 39,
    "name": "Java7"
  },
  {
    "id": 48,
    "name": "Objective-C"
  },
  {
    "id": 52,
    "name": "Pascal (fpc)"
  },
  {
    "id": 53,
    "name": "Pascal (gpc)"
  },
  {
    "id": 54,
    "name": "Perl"
  },
  {
    "id": 56,
    "name": "PHP"
  },
  {
    "id": 61,
    "name": "Python"
  },
  {
    "id": 63,
    "name": "Python 3"
  },
  {
    "id": 65,
    "name": "Ruby"
  },
  {
    "id": 71,
    "name": "SQL"
  },
  {
    "id": 75,
    "name": "VB.NET"
  }
]
```

This endpoint retrieves all the programming languages that are supported by this API.

### HTTP Request

`GET /languages`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
withVersion | 0 | if set to 1, the result will also include version information for each language.
withPopular | 0 | if set to 1, the result will include popular parameter for each language which says whether the language is popular or not (true/false).
withFileExt | 0 | if set to 1, the result will include supported file extension for each language.
onlyPopular | 0 | if set to 1, only languages that are popular will be returned.
onlyUnpopular | 0 | if set to 1, only languages that are not popular will be returned.

### Return Value
<table id="language_return_table">
  <tr>
    <td id="first_td">
      <h3>languages</h3>
      <p>array</p>
    </td>
    <td id="second_td">
      <table>
        <tr>
          <td>
            id
          </td>
          <td>
            language identifier
          </td>
        </tr>
        <tr>
          <td>
            name
          </td>
          <td>
            language name
          </td>
        </tr>
        <tr>
          <td>
            version
          </td>
          <td>
            language version (only if withVersion is set to 1)
          </td>
        </tr>
        <tr>
          <td>
            popular
          </td>
          <td>
            whether language is popular or not (only if withPopular is set to 1)
          </td>
        </tr>
        <tr>
          <td>
            fileExt
          </td>
          <td>
            supported file extension for the language (only if withFileExt is set to 1)
          </td>
        </tr>
      </table>
    </td>
  </tr>
</table>

## Get the language template

```shell
curl "http://cloudcompiler.esy.es/api/languages/template/13"
```

> The above command returns JSON structured like this:

```json
{
  "source": "#include <iostream>\nusing namespace std;\n\nint main() {\n\t\/\/ your code goes here\n\treturn 0;\n}"
}
```

This endpoint retrieves the template for the specified language.

### HTTP Request

`GET /languages/template/:id`

### URL Parameters

Parameter | Description
--------- | -----------
id | language identifier of the template to retreive

### Return Value

Parameter | Description
--------- | -----------
source | source code of the specified language template

## Get the language sample code

```shell
curl "http://cloudcompiler.esy.es/api/languages/sample/13"
```

> The above command returns JSON structured like this:

```json
{
  "source": "#include <iostream>\nusing namespace std; \/\/ consider removing this line in serious projects\n\nint main() {\n\tint intNum = 0;\n\t\n\tcin >> intNum;\n\twhile (intNum != 42) {\n\t\tcout << intNum << \"\\n\";\n\t\tcin >> intNum;\n\t}\n\n\treturn 0;\n}",
  "input": "1\n2\n10\n42\n11\n"
}
```

This endpoint retrieves the sample code for the specified language.

### HTTP Request

`GET /languages/sample/:id`

### URL Parameters

Parameter | Description
--------- | -----------
id | language identifier of the sample code to retreive

### Return Value

Parameter | Description
--------- | -----------
source | source code of the specified language sample code
input | standard input of the sample code

## Create a new submission

```shell
curl -H "Content-Type: application/json" \
-d '{ 
      "sourceCode": "# This is how submission looks like",
      "langId": 63,
      "stdin": "sample input",
      "timeLimit": 1
    }' \
-X POST \
'http://cloudcompiler.esy.es/api/submissions/'
```

> The above command returns JSON structured like this:

```json
{
  "id": "FrnO2h"
}
```

This endpoint submits the source code to the API.

### HTTP Request

`POST /submissions`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
sourceCode | null | source code need to be compiled.
langId | null | language identifier. Language identifiers can be retrieved at <a href="#get-all-supported-languages">/languages</a>.
stdin | null | data that will be given to the program on stdin.
timeLimit | 0 | when set to 0 time limit will be 5secs. When set to 1 time limit will be 15secs. Once the time limit exceeded program will be terminated.

<aside class="notice" style="text-align: center;">
    It is preferred to submit the post data in json format. However x-www-form-urlencoded is also supported.
</aside>

### Return Value

Parameter | Description
--------- | -----------
id | id of the new submission (this id will be used to retreive the status).

## Get the submission status

```shell
curl "http://cloudcompiler.esy.es/api/submissions/FrnO2h"
```

> The above command returns JSON structured like this:

```json
{
  "status": "0",
  "result": "15",
  "any_cmperr": false,
  "stdout": "",
  "stderr": "",
  "cmperr": "",
  "time": "0.02",
  "memory": "9984",
  "signal": "0"
}
```

This endpoint fetches the submission status.

### HTTP Request

`GET /submissions/:id`

### URL Parameters

Parameter | Description
--------- | -----------
id | submission identifier retreived from <a href="#create-a-new-submission">/submissions</a>

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
withSource | 0 | if set to 1, the result will also include submitted source code.
withInput | 0 | if set to 1, the result will include standard input submitted.
withLang | 0 | if set to 1, the result will include language details.
withTimestamp | 0 | if set to 1, the result will include server UNIX timestamp of submission's creation.

### Return Value

Parameter | Description
--------- | -----------
status | submission's current status (see <a href="#status">status</a> section).
result | submission's current result (see <a href="#result">result</a> section).
any_cmperr | whether there is compiler error or not (true/false).
stdout | output produced by the program.
stderr | stderr produced by the program.
cmperr | compilation error information regarding the program.
time | execution time in seconds.
memory | memory used by the program in kilobytes. 
signal | signal raised by the program when an error had occurred. 
sourceCode | source code of the submission (only if withSource is set to 1).
stdin | input data of the submission (only if withInput is set to 1).
langId | submission's language identifier (only if withLang is set to 1).
langName | submission's language name (only if withLang is set to 1).
langVersion | submission's language version (only if withLang is set to 1).
timestamp | server UNIX timestamp of submission's creation (only if withTimestamp is set to 1).

# Status

Variable status returned by the <a href="#get-the-submission-status">submissions/:id</a> method requires further explanation. 

Status specifies stage of program's execution. It's values should be interpreted as shown in the below table.

When you use the <a href="#get-the-submission-status">submissions/:id</a> method and status is not equal to 0 then you should wait 3-5 seconds and call the method again.

When the submission's status is 0 you can find out how the program has finished by checking the result variable.

Value | Meaning
------|---------
< 0 | Waiting for compilation – the submission awaits execution in the queue
0 | Done – the program has finished execution
1 | Compilation – the program is being compiled
3 | Running – the program is being executed

# Result

Variable result returned by the <a href="#get-the-submission-status">submissions/:id</a> should be interpreted in the following way:

Value | Meaning
------|---------
11 | Compilation error – the program could not be executed due to compilation error
12 | Runtime error – the program finished because of the runtime error, for example: division by zero, array index out of bounds, uncaught exception
13 | Time limit exceeded – the program didn't stop before the time limit
15 | Success – everything went ok
17 | Memory limit exceeded – the program tried to use more memory than it is allowed to
19 | Illegal system call – the program tried to call illegal system function
20 | Internal error – some problem occurred on our server; try to submit the program again and if that fails too, then please contact us