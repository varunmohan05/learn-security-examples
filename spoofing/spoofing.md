# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.

    Answer: The spoofing vulnerability in insecure.ts arises from improper session cookie configuration and lack of mechanisms to prevent Cross-Site Request Forgery (CSRF). 
Specifically:
    1. Cookies Not Marked as HttpOnly: Cookies can be accessed through JavaScript, allowing malicious actors to steal session cookies programmatically.
    2. Lack of CSRF Protection: The server does not implement CSRF tokens to verify the authenticity of requests, making it susceptible to malicious requests sent from other domains (e.g., using hidden forms in mal-csrf.html).

2. Briefly explain different ways in which vulnerability can be exploited.
    
    Answer: 
    1. **Cookie Theft**: Using the mal-steal-cookie.html page, a malicious link is hosted to lure users. When the link is clicked, JavaScript on the page accesses document.cookie to steal session cookies.
    The stolen cookies can be used to hijack the session and impersonate the user.

    2. **Cross-Site Request Forgery (CSRF)**: Using the mal-csrf.html page, a malicious form is auto-submitted to the /sensitive endpoint of insecure.ts.
    If the victim is logged into insecure.ts, the malicious request is executed with the victim's session, allowing unauthorized actions like setting sensitive data or executing operations as an admin.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.

    Answer: secure.ts mitigates the spoofing vulnerabilities using the following measures:

    1. **Secure Cookie Configuration**:
        1. httpOnly: true: Cookies cannot be accessed via JavaScript, preventing cookie theft using document.cookie.
        2. sameSite: true: Restricts cookies from being sent in cross-site requests, mitigating CSRF attacks.
    2. **Enhanced Secret Management**:
        Uses a secret passed as an argument, ensuring better security for session signing and preventing predictable cookies.

