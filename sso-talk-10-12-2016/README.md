# Digital Authentication
##### featuring the light-hearted and meandering nonsense of Nate Mielnik!



# User SSO
## Goals
* <!-- .element class="fragment" -->Single Sign On -> User enters their credentials once, and is able to be authenticated for all of our services
* <!-- .element class="fragment" -->Applications aren't require to maintain sessions server-side (but can if they want)
* <!-- .element class="fragment" -->There's a shared client library to abstract all of this away



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



# User-SSO
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


## Key Points
* Authenticate with VistaPrint, get a JWT from digital-sso
* Generate a new JWT for each application
* User JWT expires in 5 minutes, so application must create own session
* <!-- .element class="fragment" -->Questions?



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
  * <pre>http://webs-qa-digital-sso-example-springboot.us-east-1.elasticbeanstalk.com/</pre>
* Express example app
  * <pre>http://webs-qa-digital-sso-example-express.us-east-1.elasticbeanstalk.com/</pre>
* digital-sso signout
  * <pre>http://qa.oidc.digital.vistaprint.com/signout?redirect_uri=http://www.vptest.com</pre>



# Third-Party Cookies
### Fun! Fun! Fun! Now that Safari took our third-party cookies away!
<!-- .element class="fragment" --><img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/MemeThirdPartyCookies.jpg" height="400" />


## What are Third-Party Cookies (Besides Fun)?
* <!-- .element class="fragment" -->Cookies read/written within a domain different than the current site domain (ie a image or iframe points to a different domain, and that service wants to access the cookies within that domain).
* <!-- .element class="fragment" -->Problem: Safari disables third-party cookies by default. Other browsers have various states of support.
  * We can't redirect to digital-sso within the iframe
* <!-- .element class="fragment" -->Workaround: If a user has visited the domain directly in the browser, that service can read/write cookies to that domain within an image/iframe asychronous request.



# IFrame SSO
## Goals
* <!-- .element class="fragment" -->Dealing with third-party cookies since 2016!
* <!-- .element class="fragment" -->Be able to sign in and use third-party cookies to maintain session within an iframe (all of our dashboards)
* <!-- .element class="fragment" -->Have this abstracted away into the digital-sso-client and be just as easy as UserSSO to use


## 0. User visits Vistaprint.com dashboard with app1.com iframe
1. <!-- .element class="fragment" -->Does user have a cookie in app1.com domain?
2. <!-- .element class="fragment" -->If not, render a UI containing only javascript which will redirect the outer-frame to `/ssoredirect` endpoint within app1.com
3. <!-- .element class="fragment" -->Pass outer-frame url as `redirectUri`


## 1. User hits app1.com /ssoredirect
1. <!-- .element class="fragment" -->Does user have a cookie in app1.com domain?
2. <!-- .element class="fragment" -->If not, redirect to digital-sso like normal UserSSO flow, but store value of incoming `redirect_uri` separately.


## 2. User hits digital-sso /authorize
1. <!-- .element class="fragment" -->Redirect to VP Login


## 3. VP Login
1. <!-- .element class="fragment" -->Authenticate user and redirect back to `/successLogin`


## 4. Back at digital-sso service
1. <!-- .element class="fragment" -->Generate the JWT and go back to app1.com `/cb` endpoint passing the JWT in the query string (`code`)


## 5. Back at app1.com
1. <!-- .element class="fragment" -->Validate the JWT, drop a cookie in app2.vistaprint.com
2. <!-- .element class="fragment" -->If there was a specific original url stored as part of the IFrameSSO flow, redirect there


## 6. Back at Vistaprint.com dashboard with app1.com iframe
1. <!-- .element class="fragment" -->Does user have a cookie within app1.com domain?
2. <!-- .element class="fragment" -->YES! We're good to go, no redirecting necessary.



# IFrame SSO
<img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/IFrameSSOFlow.png" height="550" />


## Key Points
* Any UI that can be displayed within an iframe should use this policy/filter
* If not rendered within an IFrame, everything will work but there will be 2 extra unneccesary redirects
* <!-- .element class="fragment" -->Questions?



# SSO and user locale
## Problem:
* Vistaprint has several CCTLDs which allow us to track the user's locale
  * ie: vistprint.ca, vistaprint.fr, etc.
* <!-- .element class="fragment" -->We don't have alternate TLDs for our microservices, everything is .com
* <!-- .element class="fragment" -->When we want to redirect back to Vistaprint, we don't know their locale, so we can't pick the right CCTLD :(


## Solution: 
* Geoff, Tom, and others devised a plan to use SSO as a way to track this
* We'll track the referrer for the first time the user gets directed into a digital app (referring_host), send that to SSO, and store it in the user JWT.


## 1. Non-US Vistaprint user redirected to app1.com
#### (ie Purchase tower from vistaprint.ca)
1. <!-- .element class="fragment" -->Does user have a cookie in app1.com domain?
2. <!-- .element class="fragment" -->If not, redirect to digital-sso to get user JWT
3. <!-- .element class="fragment" -->NEW! Extract the incoming referrer and pass that as a `referring_host` query param


## 2. + 3. User hits digital-sso /authorize
1. <!-- .element class="fragment" -->If `referring_host` is present in QS, and there isn't a `referring_host` cookie set in digital-sso, write it there.
2. <!-- .element class="fragment" -->Redirect to VP Login


## 4. User hits VPLogin
1. <!-- .element class="fragment" -->Authenticate user
2. <!-- .element class="fragment" -->Redirect back to digital-sso `/successLogin`


## 5. User is back at digital-sso `/successLogin`
1. <!-- .element class="fragment" -->Verify the user authenticated with VP
2. <!-- .element class="fragment" -->Generate user JWT for app1.com
3. <!-- .element class="fragment" -->Add `referring_host` into the JWT
4. <!-- .element class="fragment" -->Redirect user back to app1.com w/ JWT


## 6. User is back at app1.com
1. <!-- .element class="fragment" -->Verify JWT is valid, create user session cookie in app1.com domain
2. <!-- .element class="fragment" -->User session contains `referring_host`, so we can use this when creating links to VP pages
3. <!-- .element class="fragment" -->User clicks a link that redirects them to VP


## 7. User is back at VP w/ original CCTLD!
<!-- .element class="fragment" --><img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/Applause.gif" />



# SSO w/ referring_host
<img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/SSOWithReferringHost.png" height="550" />


## Key Points
* Assuming we can access the original referrer the first time the user hits a digital-service, we can add this to every JWT that's generated for that user's session
* When using digital-dashboard as the main redirect hub, it's using this JWT property
* In the future, we may want to just store the locale to avoid some of the trickiness around parsing the referring_host
* <!-- .element class="fragment" -->Questions?



# Care SSO
## Goals
* <!-- .element class="fragment" -->CCT users can sign into CCT via Auth0
* <!-- .element class="fragment" -->CCT users can sign in as any user in the FIELD system, and go to applications as that user
* <!-- .element class="fragment" -->For special applications, or featuers, we can restrict access to care agents
* <!-- .element class="fragment" -->For apps that don't care about CCT, there is no special handling
  * <!-- .element class="fragment" -->JWTs look the same as regular user JWTS
  * <!-- .element class="fragment" -->digital-sso flow and user sessions work the same


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


## Key Points
* The JWT generated for impersonated users looks identical to normal user JWTs (with 2 extra claims)
* If an app wants to restrict access to just care agents, just look for existance of additional `agent_username` claim.
* <!-- .element class="fragment" -->Questions?



# Service Auth
## Goals
* <!-- .element class="fragment" -->Have a way to secure server-to-server calls from one microservice to another
* <!-- .element class="fragment" -->Avoid having to share secrets between each pair of application that calls eachother
* <!-- .element class="fragment" -->Leverage what we've done with digital-sso-clients as much as possible, so this can be abstracted away from the services


## 0. Service A wants to make request to Service B
* <!-- .element class="fragment" -->Do I have a valid JWT?
* <!-- .element class="fragment" -->If not, make request to digital-services-authentication to get a ServiceAuth JWT
* <!-- .element class="fragment" -->Generate a temporary JWT and sign it with Service A's private key


## 1. Service A requests a JWT
* <!-- .element class="fragment" -->digital-services-auth sees an incoming request, and verifies the signature on the incoming JWT to ensure its a valid request.
* <!-- .element class="fragment" -->Once incoming JWT is verified, generate a ServiceAuth JWT for Service A and return it in the response


## 2. Service A receives ServiceAuth JWT
* <!-- .element class="fragment" -->Verify returned JWT is valid, and cache it since it's good for 2 hours
* <!-- .element class="fragment" -->Generate a request to Service B, passing Service A's JWT in the Authorization Header


## 3. Service B receives request
* <!-- .element class="fragment" -->Service B receives the incoming request from Service A
* <!-- .element class="fragment" -->Check the authorization header, and verify the JWT


## 4. Service B verifies JWT
* <!-- .element class="fragment" -->Verify JWT is valid, hasn't expired, was issued for Service A, etc.
* <!-- .element class="fragment" -->Verify the signature is valid and was generated by the digital-services-auth service
* <!-- .element class="fragment" -->JWT is valid, so return response.


## 5. Service A receives response
* <!-- .element class="fragment" -->DONE!



# Service Auth
<img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/ServiceAuthFlow.png" height="550" />


## Key Points
* Application keeps its own private key, and stores its client-id + public key with central auth service
* No storing of secrets, the only thing shared across the system is the central service's public key
* Client has filter/policy for verifying incoming request + helpers for making outgoing requests + support for rolling keys
* Services can cache JWT and only request a new one when it expires
* External partners can use this, so we've started thinking about scoped JWTs, to restrict 3rd parties
* <!-- .element class="fragment" -->Questions?



# Client
* We've built a bunch of clients to allow new applications to just take an additional dependency and be able to secure endpoints quickly
* [digital-sso-client](https://github.com/websdev/digital-sso-client)
  * node/express npm module
  * sailsjs npm module
  * Java artifact
  * Future: .NET library and Ruby Gem?
* Directions for onboarding and using the client are via READMEs within the repo



# Applications
* digital-sso service
* digital-service-authentication
* examples apps
  * <!-- .element class="fragment" -->One using each version of the client (express app, sails app, spring boot app)
  * <!-- .element class="fragment" -->Very useful for all kinds of things



# Thanks!
* digital-sso: Caleb, Harbhajan, Nate, Matt Fowle, Tom Whitner, Andrew B
* digital-sso-client: Nate, Harbhajan, Thomas Gideon, Matt Halbe, Noah, Matt Fowle, Tom Whitner, Brad
* care sso: Andrew Bondarenko
* service-auth: Harbhajan, Nate