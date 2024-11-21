# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
    
    Answer: Potential vulnerabilities in insecure.ts
    1. **NoSQL Injection:**
    The username parameter from the query string is directly passed to the MongoDB query.
    This allows an attacker to inject malicious query objects, exploiting MongoDB's flexible querying syntax.
    2. **Lack of Input Validation and Sanitization:**
    The username parameter is not validated to ensure it is a valid string or sanitized to remove special characters that could be used for injection.
    3. **Overly Informative Error Handling:**
    The response reveals whether a username exists in the database or not, which can be leveraged by attackers to enumerate usernames.

2. Briefly explain how a malicious attacker can exploit them.
    
    Answer: 
    1. **Bypassing Authentication:**
    An attacker can inject a query object using MongoDB's query operators to bypass the username check: http://localhost:3000/userinfo?username[$ne]=
    $ne is the "not equal" operator in MongoDB, and this query matches all documents where username is not equal to an empty string.
    The server returns the first matching user document, exposing sensitive user information.
    2. **Username Enumeration:**
    By trying different username values in the query, an attacker can determine which usernames exist in the database based on the server's response.
    3. **Database Traversal:**
    More advanced injections could exploit other query operators (e.g., $regex, $gte) to retrieve or manipulate data.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
    
    Answer: 
    1. Input Validation:
    secure.ts checks that the username parameter is a valid string.
    This ensures non-string values or objects (e.g., query operators) cannot be processed.
    2. Input Sanitization:
    Non-alphanumeric characters are removed from the username parameter.
    This prevents injection of query operators like $ne or $regex.