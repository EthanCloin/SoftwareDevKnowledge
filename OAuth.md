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