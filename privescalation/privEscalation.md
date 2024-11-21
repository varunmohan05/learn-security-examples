# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
    Answer: 
    1. **Lack of Authentication**:
    The /update-role route does not verify if the requester is authenticated or associated with a valid user session.
    2. **Improper Authorization**:
    The system only checks the role of the userId provided in the request body to determine if the user is an admin.
    This allows malicious users to spoof the userId and escalate their privileges by providing the id of an admin user.
    3. **Insecure Input Handling**:
    The userId is taken directly from the request body and used to look up a user without validation or verification:

2. Briefly explain how a malicious attacker can exploit them.
    Answer: 
    1. **Privilege Escalation**:
    By sending a malicious POST request with the userId of an admin, an attacker can bypass the role check and update roles of other users.
    The attacker impersonates the admin by exploiting the lack of authentication and session validation.
    2. **Unauthorized Role Updates**:
    An attacker can change their role to admin and gain full control over the system, including modifying roles of other users or accessing restricted resources.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?
    Answer: 
    1. **Session-Based Authentication**:
    The /update-role route ensures the user is authenticated by checking for a valid session.
    Only logged-in users with valid sessions can perform actions.
    2. **Role-Based Authorization**:
    Authorization checks are based on the logged-in user's role, retrieved from session data.
    This ensures only authenticated admins can update roles.
    3. **Session Integrity**:
    The session cookie is configured securely:
    This prevents session hijacking and ensures that cookies are not accessible via JavaScript or cross-site requests.