---
tags:
  - toBeRefined
  - outOfStructure
---
# OAuth

OpenId Connect \- protocol allowing for federated authentication build on top of OAuth 2.0

Nonce \- arbitrary number/string which can be used only once during cryptographic communication to ensure that old communications cannot be reused in replay attacks

OAuth 1.0 \- previous version of OAuth, was more challaging in use becase of emedeed cryptography, and with more limited flow \(not really supporting other flows than server \- server\).

# Glossary

- **Authentication** \- verifying identity of the user
- **Authorization** \- verifying if identified user has access to the perform requested action 
- **Federated Authentication** \- application is relying on 3rd party service to Authenticate the users eg. LDAP, Active Directory, SAML, OpenID
- **Delegated Authorization \-** granting access to another person or application to perform actions on your behalf
- OAuth \- a protocol allowing to delegate access \(authorize\) to the resource to the 3rd party application by resource owner.

## Security

It increases security because:

- It removes the need of passing username and password to 3rd party app
- Allows for a lot of granularity of what 3rd party app is permitted to access \(scopes\)
- Access can be revoked any time

### Cryptography

- OAuth 1.0 had embedded cryptography 
- OAuth 2.0 assumes that all communication should be secured within SSL/TLS connection 
- OAuth WARP is predecessor of OAuth 1.0 which get rids of the complexity of cryptographic signatures and introduces bearer tokens instead
- When handling authentication with OAuth 2.0 you should always:
    - Use SSL/TLS
    - Check if hostname on certificate matches
    - Verify each certificate in the chain
    - Secure your CA bundle
- MAC Access Authentication
    - additional cryptographic mechanism that can be used with OAuth
    - there is additional\(?\) gmac\-sha\-1 or hmac\-sha\-256 key issued every time when `access_token`  is being issued or during registration of 3rd party app on OAuth server
    - key must be issued over secure SSL/TLS connection 
    - OAuth\-enabled API can require signature generated with the key in `Authorization` header
    - Prcess of generating signature assumes using normalized request string \(eg. nonce, HTTP method, request URI, host, port etc..\) and making signature from it
    - _Is it obligatory?_ 

## OAuth Roles

- **Resource server  \-** server hosting user\-owned resources protected by OAuth
- **Resource owner \-** user of an application, having ability to grant access to the resources he/she owns
- **Client \-** application making API requests 
- **Authorization server \-** OAuthserver responsible getting consent from the user and issuing access token which can be use by the client to access resource on behalf of resource owner 

## **Registration**

Before 3rd parrty app can be delegated with any permissions it needs to be delegated on Authorization server.

Registration is a manual process which result in issuing _Client ID_ and _Client Secret ._

# Client profiles

There are three possible client profiles:

- Server\-side web application
- Client\-side application running in the browser
- Native app

Client\-side and native app profiles, assumes that application is being run on the device of the user so issued credentials cannot be trusted to remain confidential.  For those there is not _Client Secret_ but only _Client ID_.

# Access tokens

- Most of applications are not using MAC signatures but only Bearer token
- Token must be within the request to OAuth\-enabled API in 
    - `Authorization` header
    - Query parameter
    - Form\-encoded body parameter `application/ x-www-form-urlencoded`

# Authorization flows

There are four main flows :

- Authorization code
- Implicit grant for browser\-based client\-side applications
- Resource owner password\-based grant
- Client credentials

And two more outside of main spec:

- Device profile 
    - this is additional flow for devices which do not have a web browser
    - usually user gets authorization code on the device and then needs to use the browser to provide it to OAuth server
- SAML bearer assertion profile
    - enables to exchange SAML 2.0 assertion for OAuth access token

## Authorization code flow

1. Resource owner use client app
2. Resource owner is getting redirected into the OAuth authorization server
3. OAuth authorization server authenticates user
4. OAuth authorization server asks resource owner for granting permissions to the specified scopes for 3rd party app
5. OAuth authorization server is redirecting user to the callback url and adds the `authorization_code`  in query parameter
6. Broweser app is calling web server with `authorization_code`
7. Client \(web server\) is using authorization code, `client_id`  and `client_secret`  to issue access token
8. Eventually client \(web server\) can refresh the access token by using refresh token

![image.png](image/image.png)

### Security

- This flow provides best security since `access_token` is not known even to resource owner.
- This is the only flow which allows long lasting offline access \- after issuing the token 3rd party don't need user to still have access.
- Downside is that many application will store the refresh token in the database, so it can be compromised.
- Additional `state`  attribute is passed by the client with random unique string, server should return the same value in the response, if value would be different then most likely it's due to CSRF attack
- Optionally, some applications allows implicit pre approval, so IT administrator of the organization can approve access to certain resource in advance so user can only authenticate and wouldn't be asked for the approval
- Usually when API service is getting the `access_token` it has to call OAuth Server to check it's validity, optionally instead of OAuth `access_token` some other tokens \(?\) which are signed can be used, so service can verify it's validity on its own, but then service cannot know in case access has been revoked \(it will still work till the expiration day\) 
- Access can be revoked by some kind of account managment UI on OAuth server, many OAuth providers also revokes all accesses after password is getting changed

## Implicit grant for browser\-based client\-side applications flow 

1. Resource owner use client app
2. Resource owner is getting redirected into the OAuth authorization server
3. OAuth authorization server authenticates user
4. OAuth authorization server asks resource owner for granting permissions to the specified scopes for 3rd party app
5. OAuth authorization server is redirecting user to the callback url and add `access_token`
6. Client app can use `access_token` to use an API

![image-1.png](image/image-1.png)

### Security

- Security is considered to be worse than with Server side flow since browser cannot be considered as trusted as the remote server
- Fro the otherside this flow do not consist any refresh tokens so compromising the token impact is limited to its TTL
- It's not standardize but some providers allow for immediate mode which allows to communicate in hidden iframe, so if user is already authenticated on OAuth server and approved access alreadyt then new `access_token` can issued straight away, if not the user is redirected back with an error  so application can gracefully redirect user again in visible window

## Resource owner password\-based grant flow 

1. Resource owner passes credentials to client application
2. Client app send request to OAuth server
3. OAuth server responds with `access_token` and optional `refresh_token`

![image-2.png](image/image-2.png)

### Security

- This flow exposes client credentials to the Client App so it should be used only for first party application
- This also accustoms users to passing their credentials to client apps, which is generally considered as risky practice
- `client_secret` is used in this flow  in this flow but since it's exposed to untrusty client app it cannot be considered to be really a secret

## Client credentials flow

This flow can be used if the client owns its data and doesn't need access delegation form resource\_owner. 
![image-3.png](image/image-3.png)

1. Client sends `client_id` and `client_secret`
2. OAuth server is responding with `access_token`

### Security

- Credentials should be regularly rotated 

# OAuth for mobile apps

- Mobile apps falls into two separate categories \- native apps, mobile\-optimized web apps
- Mobile optimized apps can use standar flows
- Using OAuth in native app requires embedding web view or opening system browser
- Webview
    - After response from OAuth server `access_token` can be easily extracted from the cookies store 
    - But user won't be ever logged into the OAuth server since such webview has it's own cookie store
- System browser
    - User will be still logged in to all OAuth based services
    - There must be a redirection back to native app, callback URI is then `my-mobile-app://oauth/callback`
    - `access_token` will be still in the system browser which negatively impact the security
- Native flow
    - Some OAuth providers allows also for native flow, which allows for redirecting user to `redirect_url` hosted by OAuth provider so it's displayed and can be taken programatically from body or title of the window

# OpenID Connect

- Federated identity protocol built on top of OAuth
- Next generation after original OpenID \(not built on OAuth\)

## Basic flow

1. Applications requests OAuth 2.0 authorization for scopes like `openid`, `profile`, `email`, `address` by redirecting user to an identity provider
2. After authentication user is redirected back to the client app with client\-side implicit or server side flow
3. App makes call to CheckID Endpoint for `user_id`, `aud` \(?\), `state` 
4. Optionally client app can also call `UserInfo` endpoint for more information like user's full name, picture, email address

### ID Token

- `id_token` \- the additional token which represent the user after being authenticated
    - it's not the same as `access_token` is another token which is used to obtain user profile information
    - `id_token`  is JWT token, which digitally signed and/or encrypted representation of the user's identity
    - JWT token can be cryptographically validated on the client side or sent as opaque string to `CheckID` endpoint for validation
    - Verifying JWT token on client side might increase performance \(no additional call to ClientID endpoint\) 

## Security

- Server side flow is again the most secure \- browser is only passing `authorization_code` and  server needs to use `client_id` and `client_secret` to get `id_token` and `access_code`
- On Client side flow, client app needs to use `CheckID` endpoint or decrypt JWT token to check if it has been issued for the right `issuer` \(to prevent replay attack, and malicious takeovers from another application\)

# Tools libraries

- [Google OAuth 2.0 Playground](https://developers.google.com/oauthplayground/)
