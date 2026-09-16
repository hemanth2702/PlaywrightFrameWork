# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: api\TC04_SerialApichaining.spec.ts >> This is the suite for API chaining >> GET: Get the newly added booking details
- Location: tests\api\TC04_SerialApichaining.spec.ts:50:9

# Error details

```
Error: expect(received).toBe(expected) // Object.is equality

Expected: 200
Received: 404
```

# Test source

```ts
  1  | /*
  2  | API chaining
  3  | --------------
  4  | when we send request and response comming from one api we reused as prerequisite to other api
  5  | then it is called api chaining.
  6  | 
  7  | 1. create new resource(post)--->bookingid---->sending get call(booking details)
  8  | 2. create new resource(post)--->bookingid+ token from Auth API--->full update resource(PUT)
  9  | 3. create new resource(post)--->bookingid+ token from Auth API--->partial update resource(PATCH)
  10 | 4. create new resource(post)--->bookingid+ token from Auth API--->delete resource(DELETE)
  11 | 
  12 | //temporary understanding
  13 | In playwright all test cases execute in parallel
  14 | -current test case need to execute in sequence
  15 | test.describe.serial()
  16 | 
  17 | Note:
  18 | Test case should be independent, its executing parallelly--right technique to api testing. 
  19 | 
  20 | */
  21 | 
  22 | import {test, expect} from "@playwright/test"
  23 | import { readingAPIpayload } from "../../src/utilities/readingAPI.js";
  24 | 
  25 | let baseURL= process.env.API_URL!;
  26 | let bookingId:number;
  27 | 
  28 | test.describe.serial("This is the suite for API chaining",()=>{
  29 | 
  30 |     test("POST:Create new Booking",async({request})=>{
  31 | 
  32 |         let payload = await readingAPIpayload("postdata");
  33 |         let response = await request.post(`${baseURL}/booking`,{headers:{
  34 |             "Content-Type": "application/json"
  35 |         }, data:payload});
  36 | 
  37 |         expect(response.status()).toBe(200);
  38 | 
  39 |         let jsonResponse = await response.json();
  40 |         console.log("booking created: ",jsonResponse);
  41 |         
  42 |         //id
  43 |         bookingId = jsonResponse.bookingid;
  44 |         console.log("Booking created with id: ", jsonResponse.bookingid);
  45 |         
  46 |     })
  47 | 
  48 |     //get booking details for new booking is added
  49 | 
  50 |     test("GET: Get the newly added booking details",async({request})=>{
  51 |         console.log("collecting the newly added booking details for id:...."+bookingId);
  52 |         let response = await request.get(`${baseURL}/booking/${bookingId}`);
  53 | 
> 54 |         expect(response.status()).toBe(200);
     |                                   ^ Error: expect(received).toBe(expected) // Object.is equality
  55 | 
  56 |         //json response
  57 |         let jsonResponse = await response.json();
  58 |         console.log("booking details:", jsonResponse);
  59 |         
  60 |         
  61 |     })
  62 | })
```