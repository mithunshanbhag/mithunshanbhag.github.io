---
layout: post
title:  Gotchas with JwtBearerOptions.
---

This is not much of a blog post, but more of a "note to self" (and others who might have encountered this issue).

Setup
* [Authorize] on controller action
* Auth0 access token signed with RSA SHA256 (signed with private key, verified with public).  
* Well known endpoint for jwks.
* Middleware: The ASP.NET Core JWT Bearer authentication handler should download the JSON Web Key Set (JWKS) file with the public key.


middleware setup: Example with TokenValidationParameters 
  * Show 401 Error with TokenValidationParameters in fiddler 

Correction with 
* Show response with TokenValidationParameters in fiddler 

Why does this error (with TokenValidationParameters) happen?

References
* [ASP.NET Core Web API v2.0: Authorization](https://auth0.com/docs/quickstart/backend/aspnet-core-webapi)
* [Sample from above article](https://github.com/auth0-samples/auth0-aspnetcore-webapi-samples/blob/master/Quickstart/01-Authorization/Startup.cs)
* [Decoding a JWT](https://jwt.io/)
* [jwks_uri] https://openid.net/specs/openid-connect-discovery-1_0.html
* [Navigating and JSON Web Key Sets](https://auth0.com/blog/navigating-rs256-and-jwks/)
* [JWKS](https://auth0.com/docs/jwks)
* [Verifying access tokens](https://auth0.com/docs/api-auth/tutorials/verify-access-token)

RS256 (RSA Signature with SHA-256) 