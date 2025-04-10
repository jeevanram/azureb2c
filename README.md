# Introduction
Azure AD B2C is a Customer Identity and Access Management (CIAM) solution designed to support millions of users. It enables customers to single sign-on (SSO) and seamlessly access an organization's products and services across web and mobile platforms.

It is a white-label authentication solution, allowing full customization of the user interface and user journey to reflect an organization’s branding. Every page in the sign-up, sign-in, and profile management flows can be customized using HTML, CSS, and JavaScript.

Azure AD B2C is built on the same underlying technology as Azure Active Directory, but it is tailored specifically for external customer identities. It supports a wide range of identity providers including social accounts (like Google, Facebook), enterprise accounts (via SAML or OpenID Connect), and local accounts. This allows customers to sign in with their preferred identity provider and gain single sign-on access to your applications and APIs. 

# Supported Protocols
Azure AD B2C delivers identity-as-a-service by supporting two widely adopted industry standards: OpenID Connect and OAuth 2.0.

**OpenID Connect**

OpenID Connect is an authentication protocol built on top of the OAuth 2.0 authorization framework. It enables secure user authentication for web, mobile, and desktop applications by delegating the sign-in process to a trusted identity provider—in this case, Azure AD B2C.

When integrated with Azure AD B2C, OpenID Connect allows applications to outsource user authentication and identity experiences—such as sign-up, sign-in, password reset, and profile editing—to the Azure B2C service. This reduces the complexity of handling user credentials within the application.

One of the key features of OpenID Connect is the ID token, a JSON Web Token (JWT) that contains identity information about the authenticated user. This token enables the application (client) to verify the user's identity and access basic profile information (such as name, email, or a unique user ID), all while maintaining a secure and standards-based approach to authentication

In a web application, the execution of an Azure AD B2C policy generally follows these high-level steps:

- **User accesses the web application.**
   The user navigates to a protected resource or page within the web app that requires authentication.
- **The application redirects the user to Azure AD B2C.**
  The application initiates an authentication request by redirecting the user to Azure AD B2C and specifying the user flow or custom policy to be executed (e.g., sign-up/sign-in).
- **The user completes the user flow or policy.**
  The user is guided through the configured experience—such as entering credentials, signing up, resetting a password, or selecting an identity provider.
- **Azure AD B2C issues an ID token.**
  Upon successful completion of the policy, Azure AD B2C generates an ID token (a JSON Web Token) containing claims about the authenticated user.
- **The ID token is sent to the application via the redirect URI.**
  The ID token is returned to the application through a redirect to the specified reply URL (also known as the redirect URI).
- **The application validates the ID token and creates a session.**
  The application verifies the token’s signature, issuer, audience, and expiration, then establishes a local authenticated session, typically by setting a secure session cookie.
- **The protected resource is served.**
  With a valid session in place, the application responds with the requested secure content or page.

**OAuth 2.0**

Well-suited for Single Page Applications (SPAs)
Single Page Applications (SPAs) run entirely in the browser and have different authentication needs compared to traditional server-side applications.
 - The OAuth 2.0 Authorization Code flow is recommended for SPAs. In this flow, the application receives an authorization code, which it then exchanges for access and refresh tokens.
 - The access token is used to make secure requests to protected APIs on behalf of the user.
 - The refresh token allows the application to obtain a new access token once the current one expires—without requiring the user to sign in again, enabling seamless user experiences.

**Azure AD B2C for Web Applications (OpenID Connect) and Web APIs (OAuth 2.0)**

- The web application initiates an authentication request by redirecting the user to Azure AD B2C with a specified user flow (policy), using the OpenID Connect authorization code flow.
- The user completes the interactive experience (e.g., sign-in or sign-up) defined by the Azure AD B2C policy.
- Azure AD B2C redirects the browser to the application’s redirect URI, including an authorization code (and optionally an ID token if response_type includes id_token).
- The browser forwards the authorization code (and possibly the ID token) to the web server at the redirect URI.
- The web server exchanges the authorization code with Azure AD B2C to obtain tokens by sending:
   - the authorization code,
   - the application (client) ID,
   - and the client secret or certificate (client credentials).
- Azure AD B2C returns an ID token, access token, and optionally a refresh token to the web server:
   - The ID token contains user identity information (used to establish an authenticated session).
   - The access token is used to authorize requests to the backend Web API.
   - The refresh token allows the application to silently acquire a new access token when the current one expires.
- The web server sets a session cookie to maintain the user's session.
- The web application sends API requests to the backend Web API, including the access token in the Authorization header.
- The Web API validates the access token, typically using Azure AD B2C's public keys or a middleware component.
- Upon successful validation, the Web API processes the request and returns secure data to the web application.

# User Scenarios 

**User Flows and Custom Policies in Azure AD B2C**

In Azure AD B2C, user flows and custom policies are used to define the identity experience logic that guides how users interact with the system to access your application.

- User Flows: These are predefined, built-in, and configurable identity processes provided by Azure AD B2C for common scenarios such as sign-up, sign-in, profile editing, and password reset. They are easy to configure via the Azure portal and require no code.
- Custom Policies (Identity Experience Framework): Custom policies offer advanced customization and flexibility, enabling you to design complex user journeys that are not supported by standard user flows. These are defined using XML-based configurations and allow integration with external systems, conditional logic, and advanced claims handling.

**Supported User Scenarios**

Azure AD B2C supports a wide range of identity scenarios, including:

- Sign-up and sign-in with local or social identity providers
- Combined sign-up/sign-in flow
- Password reset flows
- One-time email verification
- Multi-Factor Authentication (MFA), including:
- Conditional enforcement (e.g., based on IP address, device, or sign-in risk)
- Multiple modes: email, phone, or authenticator app


# User Migration Strategies

There are multiple strategies to migrate existing users to Azure AD B2C, depending on your user base, risk tolerance, and user experience goals:

- Seamless (Just-in-Time) Migration
   - Existing active users are migrated to Azure AD B2C without requiring any action from them.
   - On the user's first login, Azure AD B2C will validate credentials against the legacy identity provider (via API).
   - If authentication is successful, the user's password is silently updated in Azure AD B2C.
   - Subsequent logins will be handled entirely by Azure AD B2C.
   - This approach avoids forcing users to reset their password or receive migration-related emails.

- Migration with Password Reset Email
   - Users are migrated to Azure AD B2C.
   - An email is sent to users asking them to reset their password using their login email address.
   - Users must complete the password reset before accessing the application.

- Migration with One-Time Password (OTP) Option
   - Users are migrated and sent an email with an option to reset their password or use a one-time password (OTP).
   - If the user logs in using the OTP, they will be prompted to create a new password before being redirected to the application.

- No Pre-Migration – New Sign-Up Only
   - Existing users are not pre-migrated.
   - Users receive communication instructing them to sign up manually on the Azure AD B2C-powered system.

# User Journeys in AzureB2C

**Sign-up and Sign-in Flow in Azure AD B2C**

Azure AD B2C supports a combined Sign-up and Sign-in user flow, allowing users to authenticate using a variety of identity providers and scenarios:

- Sign up with an external identity provider (e.g., social accounts or federated enterprise identity systems).

- Sign in using a local (internal) account with a username and password managed by Azure AD B2C.

- Sign up or sign in using a social or third-party identity provider, such as Google, Facebook, or enterprise providers(via OpenID Connect or SAML).

- Password reset functionality is often built into the same flow or configured as a separate user journey.

**One-Time Email Verification**

Azure AD B2C supports one-time email verification for scenarios such as validating email ownership during first-time sign-in or sign-up.

 - When enabled, the user is prompted to verify their email address the first time they sign in.

 - After successful verification (via a code or link), the user is redirected to the application's home page or the originally requested resource.

**Password Complexity in Azure AD B2C**

By default, Azure AD B2C enforces strong password policies, requiring a mix of character types and lengths.
Administrators can further customize password complexity rules using custom policies to meet organizational or compliance requirements.

For full details, refer to the official documentation:
https://learn.microsoft.com/en-us/azure/active-directory-b2c/password-complexity?pivots=b2c-custom-policy

**Conditional Access with Azure AD B2C (via Azure AD integration)**

Conditional Access enables dynamic enforcement of security requirements based on user or session context. When integrated with Azure AD Conditional Access:

- MFA can be selectively enforced based on configurable conditions (e.g., user risk, device, location).

- Access can be restricted to specific applications only.

- Named locations (e.g., trusted IP ranges) can be used to allow or deny access or bypass MFA.

- Note: Conditional Access policies require integration between Azure AD B2C and a linked Azure AD tenant.
