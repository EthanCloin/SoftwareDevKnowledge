Managing work items in [GitHub Issues](https://github.com/EthanCloin/bellhop/issues)
# Roadmap:

The goal of this project is to build a fully featured authentication server. The milestones of that will be:

- Session Authentication
- JWT Bearer Authentication
- OAuth
- OIDC

_Will need to flesh out which of these are authentication methods vs frameworks vs protocols as I learn more_

## JWT Stuff

As I understand, the JWT Bearer auth method is as follows.

1. Accept username/password to the /login endpoint
2. Return an AccessToken and RefreshToken
3. Store the RefreshToken as a cookie to prevent XSS
4. Access granted to /me endpoint that requires TokenAuth
5. parameterless calll
# OAuth Stuff
[[OAuth]] is a standard that needs to be agreed upon by all involved parties in the flow. The three parties are **Client**, **Authorization Server**, and **Resource Server**.
## Client
Client is the web app which wants to access resources. It sends a request to the Authorization Server to ultimately get an AccessToken that the Resource Server can use to verify their access is valid.
## Authorization Server
This is the server that needs to implement endpoints for the Client to request access. 'Sign in with Google' is an OAuth implementation that exposes endpoints for the User to grant access then redirects back to the Client. Authorization Server is responsible for providing Access/Refresh Tokens and could implement optional extensions which could allow the Client to access identifiable User data.
## Resource Server
This is the party who has the user data which the Client wants access to. It's responsible to check that requests are only allowed to receive that data if they are properly authorized. Need more details on this, but main takeaway so far is we need a bearer AccessToken in the Authorization header.

# Resources
https://www.jwt.io/introduction#what-is-json-web-token-structure
https://www.oauth.com/