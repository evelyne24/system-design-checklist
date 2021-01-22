---
layout: default
title: Application security
parent: System security
permalink: /security/application
nav_order: 3
---

{% include toc.md %}

## Application security
{: .no_toc }

<div class="info" markdown="1">
Start by checking out the security best practices listed in the [OWASP section](/security/owasp), then the topic-based application security checklists below.
</div>

### Ensure the <mark>authentication</mark> mechanisms match the risk threshold and user experience. 

<div class="note" markdown="1">

**OAuth 2.0 or OpenID Connect**

Using an established protocol like OpenID Connect (OIDC) rather than password-based authentication offers a convenient user experience and minimises the risks associated with creating, storing and updating passwords in your own systems. 

Even the best password implementation can't prevent users from reusing passwords across multiple websites. If one of their accounts get compromised elsewhere, an attacker can still hijack their account on our system.

OpenID is a decentralised standard, so it's not controlled by any service provider like Google, Facebook, Apple etc. Passwords are never shared between websites, so if an account is compromised, changing the OpenID password immediately stops the attack from spilling over.

OIDC is an extension of OAuth 2.0 which adds login and profile information on top of the authorisation function that OAuth 2.0 has, in order to establish the _identity_ of the user, hence OIDC providers are also called Identity Providers. The [flows are similar](https://developer.okta.com/blog/2019/10/21/illustrated-guide-to-oauth-and-oidc), except that the client application receives both an _Access Token_ and an _ID token_ (a JWT token with a payload and some specific claims).

**Choose a solution in line with requirements and regulations**

Not all businesses have the same risk threshold when it comes to choosing authentication mechanisms. For example, for financial services, there are specific requirements about authenticating both end-users and system users (i.e. admins), for example [Strong Customer Authentication](https://stripe.com/en-gb/guides/strong-customer-authentication) and/or MFA with hardware to access internal systems.

</div>

- [ ] Ensure the authentication mechanism matches regulatory requirements
- [ ] When possible, prefer OpenID Connect over password-based authentication
- [ ] If you must manage passwords, enforce strong passwords with minimum length, non-repetition, special characters etc.
- [ ] Do not store passwords in plain, only one-way cryptographic hashes like `bcrypt/scrypt/PBKDF2` 
- [ ] Transmit authentication tokens only via encrypted channels, i.e. TLS


### Ensure appropriate <mark>user sessions</mark> management.

<div class="note" markdown="1">

**Token vs cookie authentication**

Traditional websites use cookie-based authentication. Modern single-page web and mobile apps, as well as internal services use API-based authentication, mostly with JSON Web Tokens (JWT). Sometimes, there's a mixed approach. The thing to remember is that if the web app needs API-based authentication, that API either has to be served from the same domain, e.g. `https://example.com/api` or the API needs [CORS](/cdn#check-if-cors-is-set-up-correctly) enabled.


**Refresh and Access Tokens**

JWT tokens can take many forms, like _Access Tokens_ in OAuth 2.0. and _Identity Tokens_ in OIDC. 

**Identity tokens** 

They contain information about the user's identity with a certain provider like Google. The token is signed with the provider's private key and the application server uses the provider's public key to verify its integrity. The same principle is applied in the enterprise version of Single Sign-On (SSO), where the SSO service signs the token with its private key. The OIDC protocol also specifies the use of specific JWT claims in the payload, like `exp`, `iss` and `aud` and a _nonce_ to prevent replay attacks. It's very hard to steal an identity token.


**Access tokens**

An access token gives a client application access to a protected resource, such as an API, on behalf of the user. An access token comes in two flavours:

1. _Reference tokens (opaque)_

- They are not tokens, but _token IDs_. The actual token is stored by a _Secure Token Service_ (STS) in a data store.
- The recipient of the token ID sends it to the STS to retrieve its contents.
- The STS offers better control over the token's lifecycle and makes it easier to revoke tokens in an emergency e.g. a lost phone or a phishing attack or invalidate tokens when users logout or uninstall an application.

2. _Bearer tokens (self-contained)_




</div>

- [ ] Invalidate sessions and delete cookies when users log out 
- [ ] Avoid forever-lasting sessions, make sessions expire often

### Provide a secure <mark>password reset</mark> flow.

<div class="note" markdown="1">

Sometimes password reset flows are treated as an afterthought and this opens up many attack vectors.

**User enumeration and privacy impact**

Never reply with "_There is no registered user with \<username\>/\<email\>_" to an account reset request. Leaking who is and who isn't a user of your business is a bad idea: 

- Firstly, it's a user privacy breach. Maybe your users don't want to be publicly associated with your company, especially for banking or dating sites.
- Secondly, an attacker can write a script to validate an entire collection of usernames and emails against your website and then spam all your users.

If the user doesn't exist or enters the wrong email or password, and the system shows an error, use a combination message like "_Your credentials don't match_". This sort of message doesn't disclose anything about the existence of the email address and doesn't make it easier for an attacker to guess which field is wrong, like in "_Your username is wrong_" or "_Your password is wrong_".


**Password reminders vs resetting passwords**

Never remind users their passwords in plain text via emails or SMS. If that password is persistent, there's a good chance that sending it through an insecure channel can lead to account theft. Someone else could glance over and memorise the user's email and password, then steal their account. Unencrypted emails and SMS are very unsecure channels. 

**Numeric reset codes**

Magic reset links are user-friendly but they don't work well in some cases. For example, the user could be signed in Chrome on their desktop, request a password reset link and then open the email on their smartphone Safari browser. This means that the session they initiate on one device is not transferable across the other device via the link. One solution for better usability is to send a numeric code and give users the option to enter a shorter numeric code on their choice of device to finish the password reset flow.

| ![Numeric reset code]({{ "/assets/images/fb_code.png" | absolute_url }} ) |
| :----: | -----|
| _Numeric reset code from Fb_ |

</div>

- [ ] Do not remind user passwords in plain text in an email or SMS
- [ ] Ask for password reset if the user has forgotten their password
- [ ] Prevent enumeration attacks and protect user privacy
- [ ] Avoid disclosing which information is wrong; instead use a combination message
- [ ] Avoid password reset emails that look like phishing; instead format them with your company brand
- [ ] Ensure relevant and readable _Sender_ and _Subject_ fields
- [ ] Embed a long reset URL inside a more user-friendly element like a button
- [ ] Provide a short, numeric code along with a magic reset link
- [ ] Ensure that the reset link has an expiry date of a day or less
- [ ] Ensure you have a mechanism to throttle password reset requests, e.g. captcha or a delay in between requests
- [ ] Set Sender Policy Framework (SPF) and Domain-based Message Authentication Reporting and Conformance (DMARC) to specify which emails servers are permitted to send email on behalf of your domain
- [ ] Avoid long delays in sending password reset emails; making users wait more than 20 seconds can impact user experience and business reputation
- [ ] Invalidate the old credentials only after the user's identity has been verified
- [ ] Always let the user know with a follow-up email when their credentials have changed

### Authorization

### Encryption in transit

### Encryption at rest

### Vulnerabilities in third-party dependencies