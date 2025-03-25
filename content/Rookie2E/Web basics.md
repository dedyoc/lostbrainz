**1. HTTP and Network Communication:**

- **HTTP vs HTTPS:**
    

|   |   |   |
|---|---|---|
|Feature|HTTP|HTTPS|
|Security|Unsecured|Secure (encrypted with SSL/TLS)|
|Performance|Slightly faster|Slightly slower (due to encryption)|
|SEO|Less favored|Favored|
|Browser Display|No secure indicator|Padlock icon, "https://"|

- **Request-Response Cycle:**
    

1. **Client Request:** The browser sends an HTTP request to the server.
    
2. **Server Processing:** The server processes the request.
    
3. **Server Response:** The server sends back an HTTP response.
    
4. **Browser Rendering:** The browser renders the response (e.g., displays a web page).
    

- **Common Request Methods:**
    

|   |   |
|---|---|
|Method|Description|
|GET|Retrieves data|
|POST|Submits data|
|PUT|Updates or creates a resource|
|DELETE|Deletes a resource|

- **Status Codes:** Examples: 200 OK, 404 Not Found, 500 Internal Server Error.
    
- **Request Structure:**
    

|   |   |
|---|---|
|Part|Description|
|Request Line|Method, URI, HTTP Version|
|Headers|Metadata about the request (e.g., User-Agent)|
|Body|Data sent with the request (optional)|

**2. Client-Side Storage:**

|   |   |   |   |
|---|---|---|---|
|Feature|Local Storage|Session Storage|Cookies|
|Scope|Origin|Origin, Session|Domain|
|Lifetime|Persistent|Session|Session/Persistent|
|Size|~5-10MB|~5-10MB|~4KB|
|HTTP Requests|Not sent|Not sent|Sent|

**3. Security:**

- **Cross-Site Request Forgery (CSRF) Protection - Workflow:**
    

1. **Token Generation:** Server generates a unique token.
    
2. **Token Embedding:** Token is embedded in a hidden form field.
    
3. **Form Submission:** User submits the form, including the token.
    
4. **Token Validation:** Server validates the token against the stored token.
    
5. **Request Processing/Rejection:** Request is processed if the token is valid, rejected otherwise.
    

- **OAuth 2.0 - Authorization Code Grant Workflow:**
    

1. **Authorization Request:** Application redirects the user to the authorization server.
    
2. **User Authorization:** User authenticates and grants consent.
    
3. **Authorization Grant:** Authorization server redirects the user back to the application with an authorization code.
    
4. **Token Request:** Application exchanges the authorization code for an access token.
    
5. **Resource Access:** Application uses the access token to access protected resources.