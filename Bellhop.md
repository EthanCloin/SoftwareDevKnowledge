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

# Resources
https://www.jwt.io/introduction#what-is-json-web-token-structure
https://www.oauth.com/