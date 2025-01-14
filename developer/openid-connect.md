---
description: >-
  OpenID Connect (OIDC) is an authentication layer built on top of the OAuth 2.0
  protocol. This guide will help you configure OIDC integration for your Teable
  application.
---

# OpenID Connect

### Environment Variables Configuration

To enable OIDC integration in Teable, set the following environment variables in your .env file:

```bash
# OIDC configuration example (using Google as the provider)
BACKEND_OIDC_CLIENT_ID=google_client_id
​
# The client secret provided by the OIDC provider (Google in this case)
BACKEND_OIDC_CLIENT_SECRET=google_client_secret
​
# The URL where the OIDC provider will redirect after successful authentication
BACKEND_OIDC_CALLBACK_URL=https://app.teable.io/api/auth/oidc/callback
​
# The endpoint URL for retrieving authenticated user information
BACKEND_OIDC_USER_INFO_URL=https://openidconnect.googleapis.com/v1/userinfo
​
# The endpoint URL for obtaining access tokens
BACKEND_OIDC_TOKEN_URL=https://oauth2.googleapis.com/token
​
# The endpoint URL where users authenticate
BACKEND_OIDC_AUTHORIZATION_URL=https://accounts.google.com/o/oauth2/auth
​
# The identifier URL of the OIDC provider (Google in this case)
BACKEND_OIDC_ISSUER=https://accounts.google.com
​
# Additional OIDC configuration options in JSON format
BACKEND_OIDC_OTHER={"scope": ["email", "profile"]}
​
# Comma-separated list of supported social authentication providers
SOCIAL_AUTH_PROVIDERS=oidc
```

### Configuration Details

* `BACKEND_OIDC_CLIENT_ID`: Your OIDC client ID for Teable, provided by your OIDC provider.
* `BACKEND_OIDC_CLIENT_SECRET`: Your OIDC client secret for Teable, provided by your OIDC provider.
* `BACKEND_OIDC_CALLBACK_URL`: The callback URL for Teable. This is set to [https://app.teable.io/api/auth/oidc/callback](https://app.teable.io/api/auth/oidc/callback). Ensure this matches the callback URL registered with your OIDC provider.
* `BACKEND_OIDC_USER_INFO_URL`: The endpoint URL for retrieving user information from your OIDC provider.
* `BACKEND_OIDC_TOKEN_URL`: The endpoint URL for obtaining access tokens from your OIDC provider.
* `BACKEND_OIDC_AUTHORIZATION_URL`: The endpoint URL where users authenticate with your OIDC provider.
* `BACKEND_OIDC_ISSUER`: The identifier URL of your OIDC provider.
* `BACKEND_OIDC_OTHER`: Additional OIDC configuration options, provided in JSON format. In this example, we're requesting "email" and "profile" scopes.

#### Enabling OIDC Authentication in Teable

To enable OIDC as an authentication method for Teable, include "oidc" in the `SOCIAL_AUTH_PROVIDERS` environment variable:

SOCIAL\_AUTH\_PROVIDERS=github,google,oidc

This will allow users to authenticate using OIDC, GitHub, and Google in Teable.

### Important Notes

1. Ensure all URLs use the HTTPS protocol for security.
2. Never hardcode these sensitive details directly into your Teable application in a production environment. Always use environment variables or a secure key management system.

Specific URLs and configurations may vary depending on your chosen OIDC provider (e.g., Google, Okta, Auth0). Refer to your OIDC provider's documentation for accurate endpoint URLs and configuration requirements.

When setting BACKEND\_OIDC\_OTHER, you may need to add additional parameters based on your requirements and what your OIDC provider supports.

5. The example in your .env.example file uses Google as an OIDC provider. If you're using a different provider, make sure to update the URLs accordingly.

By correctly configuring these environment variables, your Teable application should be able to successfully integrate OIDC authentication. If you encounter any issues, check your OIDC provider's documentation and ensure all URLs and credentials are correct.

### Example: Using Authentik as OIDC Provider

Follow these steps to configure Authentik as an OIDC provider for your Teable application:

1. Log into your Authentik admin panel and create a new OAuth2 Provider.
2.  In the OAuth2 Provider settings, set the Redirect URIs to match your `BACKEND_OIDC_CALLBACK_URL`. This should be in the format:

    ```
    https://your-teable-domain.com/api/auth/oidc/callback
    ```
3. After creating the OAuth2 Provider, you'll need to map the Authentik configuration to your Teable environment variables. Use the following correspondence:
   * Client ID → `BACKEND_OIDC_CLIENT_ID`
   * Userinfo URL → `BACKEND_OIDC_USER_INFO_URL`
   * Token URL → `BACKEND_OIDC_TOKEN_URL`
   * Authorize URL → `BACKEND_OIDC_AUTHORIZATION_URL`
   * OpenID Configuration Issuer → `BACKEND_OIDC_ISSUER`
4. Generate a client secret in Authentik and set it as `BACKEND_OIDC_CLIENT_SECRET` in your Teable environment variables.
5.  Update your Teable `.env` file with these values. Your configuration should look similar to this:

    ```bash
    SOCIAL_AUTH_PROVIDERS=oidc
    BACKEND_OIDC_CLIENT_ID=your_authentik_client_id
    BACKEND_OIDC_CLIENT_SECRET=your_generated_secret
    BACKEND_OIDC_CALLBACK_URL=https://your-teable-domain.com/api/auth/oidc/callback
    BACKEND_OIDC_USER_INFO_URL=https://your-authentik-domain.com/application/o/userinfo
    BACKEND_OIDC_TOKEN_URL=https://your-authentik-domain.com/application/o/token
    BACKEND_OIDC_AUTHORIZATION_URL=https://your-authentik-domain.com/application/o/authorize
    BACKEND_OIDC_ISSUER=https://your-authentik-domain.com/application/o/teable
    BACKEND_OIDC_OTHER={"scope": ["email", "profile"]}
    ```

Remember to replace `your-teable-domain.com` and `your-authentik-domain.com` with your actual domain names.

By following these steps, you should have successfully configured Authentik as an OIDC provider for your Teable application. Make sure to restart your Teable application after updating the environment variables for the changes to take effect.

\
