# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.
    
    Answer: 
    1. Perform actions anonymously:
    Users can send messages (/send-message) or retrieve messages (/get-messages) without authentication, making it impossible to track their identity or actions.
    2. Repudiation:
    Since there’s no record of who sent a request, malicious users can deny having performed any actions, even if they abuse the service (e.g., sending inappropriate messages or overloading the server).
    3. No Audit Trail:
    The server does not log request details such as timestamps, user identities, or IP addresses, making it impossible to trace the origin of actions.

2. Briefly explain why the vulnerability is addressed in __secure.ts__.
    
    Answer: The vulnerability is addressed in secure.ts by implementing the following defensive measures:
    1. Logging:
    The server logs all incoming requests, including method, URL, and the user’s IP address.
    This creates a detailed audit trail, ensuring that every action can be traced back to a specific user or IP.
    2. Authentication:
    The /send-message and /get-messages routes now require authentication.
    Only authenticated users are allowed to send or retrieve messages.
    3. Error Logging:
    Any errors are logged with timestamps and details.
    4. Message Tracking:
    Each sent message is logged with the user’s identity and the content.
3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?
    
    Answer: The Intercepting Filter Pattern is used to handle concerns like logging and authentication in a systematic manner.
    
    **The Intercepting Filter Pattern:**
    - Introduces a mechanism to pre-process or post-process requests and responses.
    - Applies reusable logic (e.g., logging, authentication, or validation) before or after the main business logic.

    **Implemention in secure.ts**
    1. Middleware for Logging: 
    A middleware function intercepts all requests, logs details like HTTP method, URL, and IP address, and then passes control to the next function.
    2. Middleware for Authentication:
    Authentication is checked before processing sensitive routes like /send-message and /get-messages.
    3. Error-Handling Middleware:
    Another middleware function catches errors, logs them, and sends a response.