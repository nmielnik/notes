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
}
</code></pre>


## JWT Signature
* Used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't changed along the way.
* <!-- .element class="fragment" -->Generating Signature: <pre><code>HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)</code></pre>
* <!-- .element class="fragment" -->[jwt.io debugger](http://jwt.io/) Handy app for inspecting JWT and verifying signature


## JWT.IO Debugger
<a target="_blank" href="http://jwt.io/"><img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/jwtioApp.png" height="600" /></a>



# User-SSO
<img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/SSOHowDoTheyWork.jpg" />


## 0. User visits app1.vistaprint.com
1. <!-- .element class="fragment" -->Does user have a cookie in app1.vistaprint.com domain?
2. <!-- .element class="fragment" -->If not, redirect to digital-sso service `/authorize` endpoint
3. <!-- .element class="fragment" -->Pass `state`, `nonce`, and `redirectUri` parameters


## 1. User hits digital-sso service
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
1. <!-- .element class="fragment" -->Does user have a cookie in app1.vistaprint.com domain?
2. <!-- .element class="fragment" -->No, redirect to digital-sso


## 1. User hits digital-sso service
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


## Questions?



# Care SSO
## How does it work?



# Care SSO
<img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/CareSSOFlow.png" height="550" />


## Questions?



# Service Auth
## How does it work?



# Service Auth
<img src="https://s3-us-west-2.amazonaws.com/nate-cdn/presentations/sso/ServiceAuthFlow.png" height="550" />



# Client



# Applications
* <!-- .element class="fragment" -->digital-sso service
* <!-- .element class="fragment" -->digital-service-authentication
* <!-- .element class="fragment" -->examples apps



# Thanks!
* digital-sso: Caleb, Harbhajan, Nate, Matt Fowle, Tom Whitner, Andrew B
* digital-sso-client: Nate, Harbhajan, Thomas Gideon, Matt Halbe, Noah, Matt Fowle, Tom Whitner, Brad
* care sso: Andrew Bondarenko
* service-auth: Harbhajan, Nate


