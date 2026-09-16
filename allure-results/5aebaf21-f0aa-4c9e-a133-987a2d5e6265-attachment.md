# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: api\TC04_SerialApichaining.spec.ts >> This is the suite for API chaining >> GET: Get the newly added booking details
- Location: tests\api\TC04_SerialApichaining.spec.ts:51:9

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
  27 | let token:string;
  28 | 
  29 | test.describe.serial("This is the suite for API chaining",()=>{
  30 | 
  31 |     test("POST:Create new Booking",async({request})=>{
  32 | 
  33 |         let payload = await readingAPIpayload("postdata");
  34 |         let response = await request.post(`${baseURL}/booking`,{headers:{
  35 |             "Content-Type": "application/json"
  36 |         }, data:payload});
  37 | 
  38 |         expect(response.status()).toBe(200);
  39 | 
  40 |         let jsonResponse = await response.json();
  41 |         console.log("booking created: ",jsonResponse);
  42 |         
  43 |         //id
  44 |         bookingId = jsonResponse.bookingid;
  45 |         console.log("Booking created with id: ", jsonResponse.bookingid);
  46 |         
  47 |     })
  48 | 
  49 |     //get booking details for new booking is added
  50 | 
  51 |     test("GET: Get the newly added booking details",async({request})=>{
  52 |         console.log("collecting the newly added booking details for id:....", bookingId);
  53 |         let response = await request.get(`${baseURL}/booking/${bookingId}`);
  54 | 
> 55 |         expect(response.status()).toBe(200);
     |                                   ^ Error: expect(received).toBe(expected) // Object.is equality
  56 | 
  57 |         //json response
  58 |         let jsonResponse = await response.json();
  59 |         console.log("booking details:", jsonResponse);
  60 |         
  61 |         
  62 |     })
  63 | 
  64 |     test("post:create new token for update and delete request", async({request})=>{
  65 | 
  66 |         console.log("post call to generate token.....");
  67 | 
  68 |         let payload = await readingAPIpayload("authdata");
  69 |         let response = await request.post(`${baseURL}/auth`,{headers:{
  70 |             "Content-Type": "application/json"
  71 |         },data:payload});
  72 | 
  73 |         expect(response.status()).toBe(200);
  74 | 
  75 |         //extract the token
  76 |         let jsonResponse = await response.json();
  77 |         token=jsonResponse.token;
  78 | 
  79 |         console.log(`API token is created ${token} for id ${bookingId}`);
  80 |         
  81 | 
  82 |         
  83 |     })
  84 | })
```