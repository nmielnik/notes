# "Digital SSO-my-god?!"
## Digital Authentication Systems

> An Overview of authenitcation with digital microservices

<q>featuring the incoherrent idiosyncrasies of Nate Mielnik<q>

[Digital Auth Confluence Page](https://confluence.freewebs.com/pages/viewpage.action?spaceKey=DA&title=Digital+SSO+and+Authentication)



# SSO-my-god!
## SSO has become an overloaded term
* <!-- .element class="fragment" -->Confusing mix of terms like _sso, digital-sso, ServiceAuth, digital-services-authentication, ctr, etcd, sso keys, care-sso,_ etc.
* <!-- .element class="fragment" -->User SSO is just one, user-facing flow within our services
* <!-- .element class="fragment" -->**Digital Authentication System** - a better "umbrella" term for everything


# Terminology Pt I
<dl>
  <dt>Authenticate</dt>
  <dd><!-- .element class="fragment" -->verb; prove or show (something, especially a claim or an artistic work) to be true or genuine.</dd>
  <dt>Authorize<dt>
  <dd><!-- .element class="fragment" -->verb; give official permission for or approval to (an undertaking or agent).</dd>
</dl>



# User SSO
## Goals
* <!-- .element class="fragment" -->Authenticate Vistaprint customers
* <!-- .element class="fragment" -->Single Sign On -> Users are able to authenticate to all services via one account and set of credentials
* <!-- .element class="fragment" -->Applications can identify user and ensure they're signed in without forcing them to sign-in on reach request
* <!-- .element class="fragment" -->There's a shared client library in multiple technologies to abstract all of this away


# User-SSO
## Detailed Summary
<img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/SSOHowDoTheyWork.jpg" />


## 0. User visits app1.vistaprint.com
1. <!-- .element class="fragment" -->Does user have a cookie in app1.vistaprint.com domain?
2. <!-- .element class="fragment" -->If not, redirect to digital-sso service `/authorize` endpoint
3. <!-- .element class="fragment" -->Pass `state`, `nonce`, and `redirectUri` parameters


## 1. User hits digital-sso /authorize
1. <!-- .element class="fragment" -->Store the `state` and `nonce` parameters as cookies in digital-sso domain
2. <!-- .element class="fragment" -->Redirect user to vistaprint login `authorize-account.aspx`
3. <!-- .element class="fragment" -->Pass `/successLogin` and `/failLogin` as redirect paths


## 2. VP Login
1. <!-- .element class="fragment" -->Does user have vistaprint session?
2. <!-- .element class="fragment" -->If not, prompt for their vistaprint credentials
3. <!-- .element class="fragment" -->Redirect back to `/successLogin` with a `request_token`


## 3. Back at digital-sso service
1. <!-- .element class="fragment" -->Receive `request_token` for VP Login and make server call to get `account_token` from VP
2. <!-- .element class="fragment" -->Valid `account_token` means user is signed in, now use that to request shopper info from VP
3. <!-- .element class="fragment" -->Use shopper info to request identity from the identity-service (PAID)
4. <!-- .element class="fragment" -->Generate a JWT with digital-user-id, account info, nonce (from cookie), and other data we have
5. <!-- .element class="fragment" -->Sign the JWT with the digital-sso private key
6. <!-- .element class="fragment" -->Redirect back to `redirect_uri` (from cookie) with `code` (JWT) and `state` (from cookie)


## 4. Back at app1.vistaprint.com
1. <!-- .element class="fragment" -->Verify `state` from query string is still valid
2. <!-- .element class="fragment" -->Verify `code` (JWT) is signed by the digital-sso service via public key
3. <!-- .element class="fragment" -->Verify `nonce` in JWT is valid and the JWT has not expired
4. <!-- .element class="fragment" -->User is authenticated!
5. <!-- .element class="fragment" -->Create a NEW cookie in app1.vistaprint.com with all the user data, this will be the user's session


## 0. User visits app1.vistaprint.com
1. <!-- .element class="fragment" -->Does user have a cookie in app1.vistaprint.com domain?
2. <!-- .element class="fragment" -->YES! We're good, no redirecting necessary!


## 0. User visits app2.vistaprint.com
1. <!-- .element class="fragment" -->Does user have a cookie in app2.vistaprint.com domain?
2. <!-- .element class="fragment" -->No, redirect to digital-sso


## 1. User hits digital-sso /authorize
1. <!-- .element class="fragment" -->Redirect to VP Login


## 2. VP Login
1. <!-- .element class="fragment" -->Does user have vistaprint session?
2. <!-- .element class="fragment" -->YES! Redirect back to `/successLogin` immediately


## 3. Back at digital-sso service
1. <!-- .element class="fragment" -->Generate the JWT and go back to app2.vistaprint.com


## 4. Back at app2.vistaprint.com
1. <!-- .element class="fragment" -->Validate the JWT, drop a cookie in app2.vistaprint.com
2. <!-- .element class="fragment" -->Good to go!



# User SSO
<img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/UserSSOFlow.png" height="550" />


## Summary
* Applications require SSO'd user for user-facing pages/endpoints
* Applications redirect the user to digital-sso and the user is redirected back to the app with an identifying token (JWT)
* Each app writes a domain-restricted cookie copying the user data from the token (JWT) 
* Cookie represents a session so user won't have to keep signing in for that app
* Each separate app (domain) must redirect user to digital-sso to create its own session, but this will be seamless for the user


## Terminology Pt II
<dl>
  <dt>OpenID Connect (OIDC)</dt>
  <dd><!-- .element class="fragment" -->Standardize authentication and authorization protocol digital-sso, vistaprint login, and Auth0 implement and follow</dd>
  <dt>JSON Web Token (JWT)</dt>
  <dd><!-- .element class="fragment" -->Often pronounced "Jot", JWTs are  JSON blobs containing properties about an entity in a standardized way which supports signatures</dd>
</dl>


# JSON Web Tokens
* <!-- .element class="fragment" -->"JWT" usually pronounced like "Jot"
* <!-- .element class="fragment" -->JWT is a standard with an [RFC](https://tools.ietf.org/html/rfc7519)
* <!-- .element class="fragment" -->[jwt.io](https://jwt.io/introduction/) great website with videos, documentation, etc.
* <!-- .element class="fragment" -->JWTs are encoded and contain a signature. THEY ARE NOT ENCRYPTED AND CAN BE DECODED BY ANYONE.
* <!-- .element class="fragment" -->Contains 3 main parts: Header, Payload, and Signature


## JWT Header
* JSON object with properties to describe the JWT
* Normally has "alg" and "typ" properties
* <!-- .element class="fragment" -->Example:
<pre><code>{
    "alg": "RS256",
    "typ": "JWT"
}
</code></pre>


## JWT Payload
* JSON object with Claims, which are just properties
* Reserved claims: iss (issuer), exp (expiration time), sub (subject), aud (audience), iat (issued at time)
* Private claims: any other properties we use ourselves


## Example Payload
<pre><code>{
  "nonce": "701296c064a3a80a",
  "digital_user_id": 695560,
  "shopper_id": "U444ELUTIR7JLSPNPKW0KV3F51LD7IM2",
  "first_name": "vptest",
  "last_name": "Customer",
  "email": "vptest@nmielnik.com",
  "referring_host": "undefined",
  "iat": 1476275431,
  "exp": 1476275731,
  "aud": "digital-sso-examples",
  "iss": "digital-sso",
  "sub": "695560"
}</code></pre>


## JWT Signature
* Used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't changed along the way.
* <!-- .element class="fragment" -->Generating Signature: <pre><code>HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)</code></pre>
* <!-- .element class="fragment" -->[jwt.io debugger](http://jwt.io/) Handy app for inspecting JWT and verifying signature


## JWT.IO Debugger
<a target="_blank" href="http://jwt.io/"><img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/jwtioApp.png" height="600" /></a>


## digital-sso QE public key
<pre>-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAxTNvxmhG93+quzPHQ4z8
n5gflLc+9Pu7EgbBxYUhLpWhdznY6ioUVX1wqe1YK7lxxonHqgAdDYtDor0Ta3In
5f2xxKpuh6OuD2bw+jKl9urPXZ4+HEJtcgrQ79QuxLsuGiU/BZ3Z/+ABgFZB0nFp
tB0ZFYJxbWSIFA20Mz/ho4PmYWEc31AlrqX4yaNA4WkZwFkgmU7TA0/WRZ/Sg9dt
bvfDmyr3+G034vYRVeGSKSJAXcVeALSaBp1xltXmLj1he1x3wSkW2xUDOscKJIKg
Pt3Dn2uPXbNjzgNT6JoULV1LlcM0SkeYxLFf8J3bStkPY/DuLs0ruRKjKuI12H81
cQIDAQAB
-----END PUBLIC KEY-----</pre>



# User SSO Benefits
* User only has to enter credentials once to access all services
* Cookie-sessions aren't tied to server instances or databases
* Following standardized OpenID Connect spec
* digital-sso-client provides simple support for node + springboot applications


# User SSO Complexities
* Signout requires deleting the session cookie for each application the user signed in to
* Third-party cookie restrictions make hosting application within `<iframe>` elements and signing out users complicated
* Session data size is restricted by browser cookie size limitations


# Terminology Pt III
<dl>
  <dt>digital-sso service</dt>
  <dd><!-- .element class="fragment" -->Central service for authenticating vistaprint customers that, when applications redirect users to, will redirect the user back to the application with a signed JWT containing user data</dd>
  <dt>IFrame SSO</dt>
  <dd><!-- .element class="fragment" -->Special flow to handle authenticating users for application hosted within `<iframe>` elements which are subject to browser-based third-party-cookie restrictions</dd>
</dl>


# Signout
* <!-- .element class="fragment" -->Applications store sessions in cookies in various domains, so signout is tricky
* <!-- .element class="fragment" -->Each time the user signs in to another app, we store the app's logout url in a cookie
* <!-- .element class="fragment" -->The shared client exposes a `/signout` endpoint within the app
  * <!-- .element class="fragment" -->When this endpoint is hit, the app clears the user's cookies and returns a 200


## digital-sso signout
1. <!-- .element class="fragment" -->If the user has not signed in to anything, redirect to `redirect_uri` from QS
2. <!-- .element class="fragment" -->Otherwise, render a page and pass list of signout urls to the UI
3. <!-- .element class="fragment" -->Page renders and creates an iframe for each signout url, hitting each application's signout endpoint
  * <!-- .element class="fragment" -->This allows the app to clear cookies in that domain
4. <!-- .element class="fragment" -->Once all signout endpoints respond, or the timeout is hit, redirect to `redirect_uri` from QS


## User SSO DEMO (Signin + Signout)
* SpringBoot example app
  * <pre>http://qa.springdigitalsso.digital.vistaprint.io</pre>
* Express example app
  * <pre>http://qa.expressdigitalsso.digital.vistaprint.io</pre>
* digital-sso signout
  * <pre>https://qa.oidc.digital.vistaprint.com/signout?redirect_uri=http://www.vptest.com</pre>



# Care SSO
## Goals
* <!-- .element class="fragment" -->CARE can authenticate to CCT through Auth0
* <!-- .element class="fragment" -->CCT users can sign in as any Vistaprint customer and visit applications as that corresopnding digital user
* <!-- .element class="fragment" -->For special applications, or featuers, we can restrict access to care agents
* <!-- .element class="fragment" -->For apps that don't care about CCT, there is no special handling
  * <!-- .element class="fragment" -->JWTs look the same as regular user JWTS
  * <!-- .element class="fragment" -->digital-sso flow and user sessions work the same


# Care SSO
## Detailed Summary


## 0. CCT User visits CCT Dashboard
* <!-- .element class="fragment" -->If there isn't a cookie for the user, redirect directly to special `/impersonate` endpoint in digital-sso
* <!-- .element class="fragment" -->Info for which user to impersonate is passed via qs (ie shopper id, email, etc.)


## 1. digital-sso /impersonate
* <!-- .element class="fragment" -->Store passed user data in a temporary cookie
* <!-- .element class="fragment" -->Redirect user to auth0 with `/callbackAuth0` as the success redirect url


## 2. Auth 0
* <!-- .element class="fragment" -->User authenticates with their CCT credentials
* <!-- .element class="fragment" -->Redirected back to digital-sso `/callbackAuth0` with `code`


## 3. digital-sso /callbackAuth0
* <!-- .element class="fragment" -->Use `code` parameter passed to get a valid token from Auth0
* <!-- .element class="fragment" -->Once validated, generate a JWT with temporary cookie data from before
* <!-- .element class="fragment" -->Redirect back to CCT dashboard with `code` query param


## 4. Back at CCT Dashboard
* <!-- .element class="fragment" -->Verify the code query param, and store a cookie to remember the user is signed in


## 0. CCT user visits app1.vistaprint.com
* <!-- .element class="fragment" -->Follow User SSO flow, no coookie, so redirect to digital sso


## 1. CCT user hits digital-sso /authorize
* <!-- .element class="fragment" -->Check for existance of care-sso cookies
* <!-- .element class="fragment" -->Since cookies exist, just generate a new JWT and redirect back to the application


## 2. Back at app1.vistaprint.com
* <!-- .element class="fragment" -->Everything looks the same as the normal user flow, so create session for impersonate user



# Care SSO
<img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/CareSSOFlow.png" height="550" />


## Summary
* The JWT generated for impersonated users looks identical to normal user JWTs (with 2 extra claims)
* If an app wants to restrict access to just care agents, just look for existance of additional `agent_username` claim.
* <!-- .element class="fragment" -->Questions?


# Terminology Pt IV
<dl>
  <dt>Care SSO</dt>
  <dd><!-- .element class="fragment" --></dd>
  <dt>impersonate (digital-sso)</dt>
  <dd><!-- .element class="fragment" --></dd>
</dl>



# Service Auth
## Goals
* <!-- .element class="fragment" -->Have a way to secure server-to-server calls from one microservice to another
* <!-- .element class="fragment" -->Avoid having to share secrets between each pair of applications that call eachother
* <!-- .element class="fragment" -->Leverage what we've done with digital-sso-clients as much as possible, so this can be abstracted away from the services


# digital services authentication
* https://github.com/websdev/digital-services-authentication
* <!-- .element class="fragment" -->Central service that has a public/private key relationship with each digital microservice
* <!-- .element class="fragment" -->Only one location that generates JWTs for apps to use when calling other apps


# Service Auth
## Detailed Summary


## 0. Service A wants to make request to Service B
* <!-- .element class="fragment" -->Do I have a valid JWT?
* <!-- .element class="fragment" -->If not, make request to digital-services-authentication to get a ServiceAuth JWT
* <!-- .element class="fragment" -->Generate a temporary JWT and sign it with Service A's private key


## 1. Service A requests a JWT
* <!-- .element class="fragment" -->digital-services-auth sees an incoming request for a client: Service A
* <!-- .element class="fragment" -->Retrieve public key for Service A and verifies the signature on the incoming JWT
* <!-- .element class="fragment" -->Once incoming JWT is verified, generate a ServiceAuth JWT for Service A and return it in the response


## 2. Service A receives ServiceAuth JWT
* <!-- .element class="fragment" -->Verify returned JWT is valid, and cache it since it's good for 2 hours
* <!-- .element class="fragment" -->Generate a request to Service B, passing Service A's JWT in the Authorization Header


## 3. Service B receives request
* <!-- .element class="fragment" -->Service B receives the incoming request from Service A
* <!-- .element class="fragment" -->Check the authorization header, and verify the JWT contained within it


## 4. Service B verifies JWT
* <!-- .element class="fragment" -->Verify JWT is valid, hasn't expired, was issued for Service A, etc.
* <!-- .element class="fragment" -->Verify the signature is valid and was generated by the digital-services-auth service
* <!-- .element class="fragment" -->JWT is valid, so return response.


## 5. Service A receives response
* <!-- .element class="fragment" -->DONE!



# Service Auth
<img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/ServiceAuthFlow.png" height="550" />


## Key Points
* Applications keep their own private key, no sharing!
* Applications store their `client_id` and public key in DSA config (microservice toolkit)
* Only thing shared across the system is the DSA public key (no secrets)
* Client has filter/policy for verifying incoming request + helpers for making outgoing requests + rolling keys
* Services can cache JWT and only request a new one when it expires
* External partners can use this, so we've started thinking about scoped JWTs, to restrict 3rd parties
* <!-- .element class="fragment" -->Questions?


# Terminology Pt V
<dl>
  <dt>ServiceAuth</dt>
  <dd><!-- .element class="fragment" --></dd>
  <dt>digital-services-authentication service</dt>
  <dd><!-- .element class="fragment" --></dd>
</dl>


# Terminology Pt VI
<dl>
  <dt>Signature/Signed</dt>
  <dd><!-- .element class="fragment" --></dd>
  <dt>Encrypted/Encrypt</dt>
  <dd><!-- .element class="fragment" --></dd>
  <dt>Decrypted/Decrypt</dt>
  <dd><!-- .element class="fragment" --></dd>
</dl>


# Terminology Pt VII
<dl>
  <dt>Private Key</dt>
  <dd><!-- .element class="fragment" --></dd>
  <dt>Public Key</dt>
  <dd><!-- .element class="fragment" --></dd>
</dl>


# Terminology Pt VIII
<dl>
  <dt>digital-sso public key</dt>
  <dd><!-- .element class="fragment" --></dd>
  <dt>digital-services-authentication public key</dt>
  <dd><!-- .element class="fragment" --></dd>
</dl>


# Terminology Pt IX
<dl>
  <dt>application private key</dt>
  <dd><!-- .element class="fragment" --></dd>
  <dt>ecnrypted session</dt>
  <dd><!-- .element class="fragment" --></dd>
</dl>


# Terminology Pt X
<dl>
  <dt>rolling keys</dt>
  <dd><!-- .element class="fragment" --></dd>
  <dt>example apps</dt>
  <dd><!-- .element class="fragment" --></dd>
</dl>



# digital-sso-client
* We've built a few clients to allow new applications to just take an additional dependency and be able to secure endpoints quickly
* [digital-sso-client](https://github.com/websdev/digital-sso-client)
  * node/express npm module
  * sailsjs npm module
  * Java artifact
  * Future: .NET library and Ruby Gem?
* Directions for onboarding and using the client are via READMEs within the repo



# example apps


# Thanks!
* digital-sso: Caleb, Harbhajan, Nate, Matt Fowle, Tom Whitner, Andrew B
* digital-sso-client: Nate, Harbhajan, Thomas Gideon, Matt Halbe, Noah, Matt Fowle, Tom Whitner, Brad
* care sso: Andrew Bondarenko
* sso w/ locales: Geoff, Tom Whitner
* service-auth: Harbhajan, Nate