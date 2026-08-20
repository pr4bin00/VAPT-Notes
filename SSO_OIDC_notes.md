# SSO/OIDC Integration VAPT Technical Notes

### 1.1 State Parameter Validation (Login CSRF)
*   **Test Action:** Initiate a login sequence. Intercept the final authorization code callback sent from Zitadel to the application endpoint (`/callback?code=xyz&state=123`). Modify or completely delete the `state` value before letting the request reach the app.
  

### 1.2 PKCE Protocol Downgrade
*   **Test Action:** Intercept the initial `/oauth/v2/authorize` request and drop both the `code_challenge` and `code_challenge_method` parameters to force a fallback to traditional OAuth 2.0.


### 1.3 Implicit Flow Downgrade
*   **Test Action:** Manipulate the initial OIDC authorization URL by changing the `response_type=code` parameter to implicit parameters (`token` or
 `id_token token`) to attempt to leak tokens directly into the browser URL fragment.


### 1.4 Redirect URI Whitelist Bypass
*   **Test Action:** Target the `redirect_uri` parameter during the initial authorization handshake by injecting external hosts, path traversal payloads (`/../../`), and URL parser credentials (`https://fontana-ai.com@://attacker.com`).

### 1.5 Authorization Code Replay
*   **Test Action:** Capture an active authorization code from a successful callback sequence and immediately resubmit it to the server-to-server token exchange endpoint a second time via Burp Suite.

### 1.6 Scope Escalation
*   **Test Action:** Append unrequested profile data variables listed in the openid-configuration (`+phone+address`) directly to the initial handshake `scope` string to test for excessive data disclosure.

### 1.7 Pre-Authentication Account Takeover
*   **Test Action:** Evaluate if an unverified or pending user registration state can be leveraged to pre-hijack an account workspace or if an uncompleted administrator invitation flow leaks dashboard access.
