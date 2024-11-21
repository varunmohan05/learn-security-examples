# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.
    
    Answer: 
    1. **Lack of Input Validation**:
    The id parameter is directly used in the database query without validation.
    Malformed or malicious input (e.g., id[$ne]=) can cause the server to process expensive or invalid database operations, consuming resources or crashing the server.
    3. **No Rate Limiting**:
    The server does not limit the number of requests from a single IP. An attacker can flood the endpoint with requests, overwhelming the server and causing legitimate users to experience downtime.
    4. **Improper Error Handling**:
    If a database query throws an exception (e.g., due to invalid _id formats), the server does not handle it gracefully, leading to potential crashes or high memory consumption.

2. Briefly explain how a malicious attacker can exploit them.
    
    Answer:
    1. **Request Flooding**:
    Without rate limiting, an attacker can send a large number of requests in a short time, causing a Denial-of-Service (DoS) for legitimate users.
    2. **Server Crashes**:
    Invalid input, such as a malformed or non-string _id parameter, can cause unhandled exceptions in the database query, potentially crashing the server. 

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?
    
    Answer:
    1. **Rate Limiting**:
    secure.ts uses the express-rate-limit middleware to limit the number of requests from a single IP:
    This prevents attackers from overwhelming the server with a flood of requests.
    2. **Error Handling**:
    Catches and logs database query errors to prevent server crashes:
    This ensures that unexpected inputs do not disrupt server operations.
    3. **Request Message Throttling**:
    Instead of overloading the server with multiple simultaneous operations, the application responds with a clear message ("Server is busy") when too many requests are received.