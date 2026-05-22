Need to learn the OAuth2.0 protocol, reading through a public resource [oauth.com](https://www.oauth.com/)to better understand
blog post from guide author: https://aaronparecki.com/oauth-2-simplified/

OAuth 2.0 is a delegation framework, allowing third-party apps to act on behalf of a user, without needing to know the user's identity. **Authorization** not Authentication.
# Notes on the Guide
# Background
pre OAuth we let third-party services have user passwords. that's risky.
big tech companies implemented their own solutions, not a unified interface.
OAuth 1 spec went public August 2007, offering standard for API access control.

OAuth 2.0 development had conflict between web and enterprise goals.
That conflict led to much core behavior being split between draft versions. They changed the verbiage for the core document from a "protocol" to "framework". [[Protocol vs Framework]]

This guide will provide concrete recommendations that most web services have coalesced to include.

# Getting Ready
## Creating an Application
Every OAuth 2.0 service makes you register an application to receive a ClientID and ClientSecret.
Also register redirect URLs, which the OAuth 2.0 service will send users after authentication.
## Redirect URLs and State
Redirects must be HTTPS. Avoid parameters, just provide a path.
You can pass a 'state' param to the OAuth 2.0 service. That param is not accessed by the OAuth 2.0 service and can include an encrypted redirect value that the client app can route to after the OAuth 2.0 service completes auth and goes to the registered redirect location. 
# Accessing Data in an OAuth 2.0 Server
## Create an Application
Directs us to GitHub to register an application with them.
## Setting up the Environment
Directs us to make a PHP single file app that uses application client id+secret to authorize with GitHub.
## Authorization Request
Build a request to GitHub including these params:
- `response_type`
- `client_id`
- `redirect_uri`
- `scope`
- `state`
Response will include `code` and `state` params.
## Obtaining an Access Token
validate that the state param matches the value our client app provided before continuing to protect against CSRF.
Then exchange the provided authorization code for an Access Token. Save that token in the session.
## Making API Requests
Future requests should include the Access Token in the Authorization header, preceded by 'Bearer '
# Signing in with Google
OAuth 2.0 is for authorization, not authentication. (hmm didn't realize that)
The end result of every OAuth flow is an accesstoken that allows modification, saying nothing about who the user is.

A standard approach to authentication is [[OpenID Connect]], an extension of OAuth2
## Create an Application
Directions to go to google developers console and create an application w clientid and secret.
## Setting up the Environment
Directs us to make a PHP app with the ID and Secret + authorize/token urls from google.
## Authorization Request
very similar to previous OAuth 2.0 flow, different `scope='openid email'`
we still get a `code` and `state` response.
## Getting an ID Token
Verify the `state` val matches.
Open ID Connect endpoint will return an Access Token and an ID Token.
ID Token is a Base64 encoded JWT.
## Verifying the User Info
we can extract data from the ID Token like the `sub` subject property. that contains the Unique Identifier for the signed in user. Store that in the session.
we can get userinfo from the Google api using their /userinfo endpoint and providing our AccessToken as the bearer token.
# Server-Side Apps
Most common app that interacts with OAuth servers. Browser requests to the web server who makes a request to the authorization server. 
Use the authorization_code grant type which we can exchange for an access token. 
# Authorization Code Grant
User authorizes and is redirected with a temporary code. Application can exchange that for an Access Token
Recommended that all OAuth services use [[PKCE]] security standard. This is where 'challenge' comes into play, need to dig into that.
## Example Flow
App initiates by building a URL containing client ID, scope, state and PKCE code verifier. This goes in an anchor tag so the browser will redirect user to the OAuth server.
User approves. OAuth server redirects back to the app including `code` in the URL.
App exchanges `code` for an AccessToken.
## Possible Errors
Upon error, the redirect URL always contains a `error` param and may include `error_description` or `error_uri`
## User Experience Considerations
Authorization page cannot be an embed or iframe. User couldn't verify against phishing.
Native mobile apps and SPAs don't require `client_secret` param bc they can't protect that value.
# Single-Page Apps
Best option for SPAs is to utilize PKCE extension.
SPAs will communicate directly with the OAuth server.
## Authorization
App sends user to `/authorize` endpoint of OAuth server
### Authorization Grant Parameters
- `response_type`
- `client_id`
- `redirect_uri`
- `scope`
- `state`
App exchanges `code` in response for AccessToken
## Implicit Flow
This protocol was made to get around a former limitation of browsers. Previously, could not make requests to other domains. Now we have CORS config to allow that.
## Security Considerations for Single-Page Apps
Cross Site Scripting (XSS) is a concern in SPAs. Policies around token lifetimes can help mitigate risk here.
### Refresh Tokens
A stolen RefreshToken is more dangerous than a stolen AccessToken. 
If the OAuth server permits sending a RefreshToken to a SPA, must abide by a few guidelines to mitigate risk.
- Each RefreshToken is valid for only one use.
- Auth server must issue a new RefreshToken each time one is utilized to get a new AccessToken.
### Storing Tokens
We need to hang onto the Access/Refresh Tokens. Can go in LocalStorage but then any other app can access those values. (what about Http-Only cookies?)
### Choosing an Alternative Architecture
If you have a web server backend in C# or Java or whatever, use that to handle the OAuth flow. You can safely store a secret and eliminate a lot of vulnerabilities.
# Mobile and Native Apps
i'm skipping this for now. only care ab web atm.
# Making Authenticated Requests
Put the AccessToken in the Authorization header
Don't try to parse the AccessToken, consider it opaque to the client.
Client does need to handle the refresh flow when the AccessToken expires.
## Refresh Tokens
kinda already talked ab this
## Registering an Application
Now we are thinking about what an implementation of an OAuth server is responsible for. Clients who want access must have the ability to register an application and generate their client_id and client_secret. 

Different implementations vary, but keep in mind that mobile+SPA clients should not have a client_secret assigned. 
## Client ID and Secret
Protect the secret judiciously. Obscure it in the webpage, only save hashed/encrypted versions.
## Deleting Applications and Revoking Secrets
Deleting an application should immediately **revoke all access tokens**.
Revoking a secret is the process a developer can use to reset an exposed client_secret.

