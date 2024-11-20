# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

    Answer: Potential vulnerabilities in insecure.ts:
    1. **Lack of Input Sanitization:**
        User inputs (e.g., req.body.name) are directly used to update the session and generate the HTML response without sanitization. This makes the application vulnerable to Cross-Site Scripting (XSS) attacks.
    2. **Dynamic HTML Rendering:**
        Unsanitized user inputs are injected into the HTML response (res.send()), enabling an attacker to insert malicious scripts into the rendered webpage.

2. Briefly explain how a malicious attacker can exploit them.
    Answer: 
    1. **Cross-Site Scripting (XSS):**
    By entering a malicious script in the name field, such as the one in readme.
    The browser executes the injected script when the page reloads, leading to potential exploits like:
        - Defacing the site (e.g., replacing content with malicious links).
        - Redirecting users to phishing or malicious sites.
    2. **Persistent XSS:**
    If the req.session.user value is stored and displayed on multiple pages, the malicious script persists across sessions, affecting every page that displays the injected name.

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?
    Answer: 
    1. **Input Sanitization with escapeHTML:**
        secure.ts uses the escapeHTML function to sanitize user inputs before processing or rendering them. This function replaces special HTML characters (<, >, &, ", ') with their escaped equivalents, effectively neutralizing malicious scripts.
        For example:
    2. **Proper Input Handling:**
        Instead of directly injecting user input into the HTML response, secure.ts ensures that any potentially dangerous characters are escaped, preventing the browser from interpreting them as executable code.