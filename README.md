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
