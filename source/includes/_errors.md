# Errors

> Example error response:

```json
{
  "error": "SYSTEM_ERROR",
  "errorCode": 500,
  "errorDesc": "Some system error occurred, Please try again."
}
```

Whenever there is an unhandled exception in any of the method instead of their original output data following data will be returned.

Parmeter | Value
---------|--------
error | name of the error
errorCode | a 3 digit number to identify the number
errorDesc | small description about the error

<aside class="notice" style="text-align: center;">
    Note that error codes given below are different from the http error codes
</aside>

Given below is the list of all possible errors identified by their error codes.

Error Code | Meaning
-----------|---------
400 | Some unknown error occurred.
500 / 501 / 502 / 503 | Something wrong happened in the server.
600 | Provided input for time limit is not valid.
601	| Language with specified id is not available.
602	| Submission with a specified id could not be found.
603	| No template available for the specified language id.
604	| No sample code available for the specified language id.
700	| Unable to find the source code in the input parameters.
701	| Unable to find the language id in the input parameters.

<aside class="success" style="text-align: center;">
  While handling errors, first check the http code. If it is non-200 then handle the error distinguishing with the help of the error data returned.
</aside>

<aside style="text-align: center; margin-top: 35px; margin-bottom: 25px; font-weight: 600; background: transparent;">
    &copy; 2016 <a href="https://github.com/harish095">Harish Kandala</a>
</aside>