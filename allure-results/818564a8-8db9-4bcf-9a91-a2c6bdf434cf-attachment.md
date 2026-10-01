# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: api\TC07_SchemaValidation.spec.ts >> test for restbooker schema validation
- Location: tests\api\TC07_SchemaValidation.spec.ts:9:6

# Error details

```
SyntaxError: Unexpected token '<', "<html>
<he"... is not valid JSON
```

# Test source

```ts
  1  |  import {expect, test} from "@playwright/test"
  2  |  import Ajv from "ajv"
  3  | import { readingAPIpayload } from "../../src/utilities/readingAPI.js";
  4  | 
  5  |  const ajv=new Ajv();
  6  | 
  7  |  const BaseUrl=process.env.APP_URL!;
  8  | 
  9  |  test("test for restbooker schema validation",async({request})=>{
  10 | 
  11 |     //send request and get the response
  12 |     const payload = await readingAPIpayload("postdata");
  13 |     const response = await request.post(`${BaseUrl}/booking`,{headers:{'Content-Type':'application/json'},data:payload})
  14 | 
  15 |     //get then json response
> 16 |     let jsonResponse = await response.json();
     |                        ^ SyntaxError: Unexpected token '<', "<html>
  17 |     //console.log(jsonResponse);
  18 |     
  19 | //store schema: https://transform.tools/json-to-json-schema
  20 | let schema={
  21 |   "type": "object",
  22 |   "properties": {
  23 |     "bookingid": {
  24 |       "type": "number"
  25 |     },
  26 |     "booking": {
  27 |       "type": "object",
  28 |       "properties": {
  29 |         "firstname": {
  30 |           "type": "string"
  31 |         },
  32 |         "lastname": {
  33 |           "type": "string"
  34 |         },
  35 |         "totalprice": {
  36 |           "type": "number"
  37 |         },
  38 |         "depositpaid": {
  39 |           "type": "boolean"
  40 |         },
  41 |         "bookingdates": {
  42 |           "type": "object",
  43 |           "properties": {
  44 |             "checkin": {
  45 |               "type": "string"
  46 |             },
  47 |             "checkout": {
  48 |               "type": "string"
  49 |             }
  50 |           },
  51 |           "required": [
  52 |             "checkin",
  53 |             "checkout"
  54 |           ]
  55 |         },
  56 |         "additionalneeds": {
  57 |           "type": "string"
  58 |         }
  59 |       },
  60 |       "required": [
  61 |         "firstname",
  62 |         "lastname",
  63 |         "totalprice",
  64 |         "depositpaid",
  65 |         "bookingdates",
  66 |         "additionalneeds"
  67 |       ]
  68 |     }
  69 |   },
  70 |   "required": [
  71 |     "bookingid",
  72 |     "booking"
  73 |   ]
  74 | }
  75 | 
  76 | 
  77 | //validation of response with schema
  78 | 
  79 | let validate = ajv.compile(schema);//this method return anonymous function whose name is validate.
  80 | let isValid = validate(jsonResponse);
  81 | 
  82 | if(!isValid)
  83 | {
  84 |     console.log("Schema Error: ", validate.errors);
  85 |     
  86 | }
  87 | 
  88 | expect(isValid).toBeTruthy();
  89 | console.log("response is valid as per schema");
  90 | 
  91 | 
  92 | })
```