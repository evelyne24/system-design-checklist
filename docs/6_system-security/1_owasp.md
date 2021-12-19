---
layout: default
title: OWASP
parent: System security
permalink: /security/owasp
nav_order: 1
---

{% include toc.md %}

## OWASP guidelines
{: .no_toc }

<div class="info" markdown="1">
The [Open Web Application Security Project (OWASP)](https://owasp.org/www-project-top-ten/) is an Open Source, non-profit organisation dedicated to improve software security. 

[OWASP Top Ten](https://owasp.org/www-project-top-ten/) guidelines is the _de facto_ web security checklist and should be consulted regularly for new updates.
</div>

### Check if <mark>SQL Injection (SQLi)</mark> protection has been applied.

<div class="note" markdown="1">
Injection can happen in more than just SQL, for example OS commands, SMTP headers, LDAP (accessing directory services), XML parsers, Stored Procedures etc.

Injection can result in data theft, data loss and even DOS attacks. It has severe cost and legal implications on the business and must be treated carefully, as soon as possible in the system design process.

**Positive vs negative security**

Negative security means disallowing certain dangerous characters and patterns. For example, even with an ORM an attacker can use a single-quote `'` to hijack a query like this:

```js
sequelize.query("SELECT FROM products WHERE name LIKE ('%" + req.params.name + "%')", 
{ type: sequelize.QueryTypes.SELECT } );
```

An attacker can send a `name` parameter with `') UNION SELECT CONCAT('username', '_', 'password') FROM users --`, which would make the resulting query:

```sql
SELECT FROM products WHERE name LIKE ('%') UNION SELECT CONCAT('username', '_', 'password') FROM users -- %'`.
``` 

The `--` comments out the rest of the text to keep the SQL query intact.

Positive security means only allowing certain character ranges like letters, digits and spaces, also known as _whitelisting_. It's preferrable to add new characters explicitly rather than allowing them implicitly but the drawback is that some Unicode characters with legitimate use cases are hard to expand to. 

A better way to write the query above with the ORM is to use a _Parameterized Statement_ (`?`) which the ORM understands to escape as a non-SQL command. 


```js
sequelize.query("SELECT FROM products WHERE name LIKE ?", 
{ 
    replacements: ['%' + req.params.name + '%'], 
    type: sequelize.QueryTypes.SELECT
})
```

Even better is to use the model query rather than raw `SELECT` queries:

```js
Product.findAll({
    attributes: {
        name: {
            [Op.like]: '%' + req.params.name + '%'
        }
    }
})
```
</div>

- [ ] Validate, filter and sanitise user input in the application server
- [ ] Disallow dangerous characters or paterns like `'`, `\b`, `\`, `x00` 
- [ ] Limite character ranges like `/a-zA-Z0-9/` (preferrable when possible)
- [ ] Use Parameterized Statements and Object Relational Mapping Tool (ORM) 
- [ ] Consider using `LIMIT` to control against mass disclosure of SQL records in case of injection
- [ ] Scan ORM packages regularly against [Common Vulnerabilities and Exposures](https://cve.mitre.org/)
- [ ] Enable web firewall with managed rules against common exploits in your CDN to block malicious requests 

### Check if <mark>Cross-Site Scripting (XSS)</mark> protection has been applied.

<div class="note" markdown="1">
XSS vulnerabilities allow attackers to capture user information and inject malicious code into an exposed web application, or even re-write the entire HTML page. Some examples include

- hijacking user sessions and redirect them to unwanted sites 
- extracting sensitive information from session tokens or cookies
- if the user is an admin, the attack could extend to the server-side and the database

The most common attack vectors are `<script>`, `<body>`, `<iframe>`, `<img>`. `<input>`, `<link>`, `<object>` tags and JavaScript events like `onerror` and `onload`.

There are [three types of XSS attacks](https://owasp.org/www-community/Types_of_Cross-Site_Scripting):

1. **Reflected XSS attacks**

A malicious script is injected into a HTTP request which is then reflected from the server in a HTTP response and executed in the victim's browser. For example, say that for a request like `https://example.com/profile?user=Alex` the server responds with "_Welcome back, Alex_". 

An attacker can hijack that URL with `https://example.com/profile?<script>something bad</script>`. When the response is sent back from the server, instead of "_Welcome back, \<name\>_", the malicious script is executed instead.

2. **Stored XSS attacks**

Malicious content finds its way on the server through unproteted user input like a comment field and gets stored in the database. Then the content is inserted unfiltered and unencoded in the website, leading to all users becoming victims.

The best practices are listed in the [XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html). 

3. **DOM-based XSS attacks**

The vulnerability lies in the browser-side script code and can take many forms, like replacing a DOM element with malicious login panel, stealing key strokes, download software etc.

The [DOM-based XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html) has the best practices.

These three types of attacks can overlap into:

- Client-side XSS
- Server-side XSS

| ![Types of XSS](https://owasp.org/www-community/assets/images/Server-XSS_vs_Client-XSS_Chart.png) |
| :-----: | ---- | 
[Types of XSS](https://owasp.org/www-community/Types_of_Cross-Site_Scripting) |

</div>

- [ ] Escape and html-encode user input
- [ ] Validate known types of input, e.g. phone numbers can contain digits, dashes and parantheses
- [ ] Filter out hazardous characters like `\`, `<>`, `()`, `+` etc. from input fields
- [ ] Use a well-maintained security encoding library rather than manually crafted rules
- [ ] Avoid inserting untrusted data into common attack vector tags and events described above
- [ ] Implement both client and server input validation
- [ ] Use a code-scanning tool to detect vulnerabilities during CI/CD
- [ ] Implement automated testing to reveal potential vulernabilities
- [ ] Use `Cross Origin Resource Sharing` (CORS) headers appropriately
- [ ] Use `Content-Security-Policy` (CSP) headers appropriately

### Ensure <mark>Cross-Site Request Foregery (CSRF)</mark> vulnerabilities have been considered.

<div class="note" markdown="1">
CSRF is a type of attack where the browser is tricked to execute an unexpected function (e.g. transfer funds or purchase something) on a website _where the user is logged in_. It exploits the fact that the website implicitly trusts a logged in user. 

A CSRF is more elaborate to pull off, usually requiring social engineering, and only allows a state change but it cannot receive the content of the HTTP response. 

The attack has two parts:

1. Trick a user into clicking on a malicious link or loading a page.
2. Send a legitimate-looking request from the victim's browser to the website, using cookies associated with the user session but request values chosen by the attacker. 

The forged request can sometimes happen through a `GET` because some vulnerable websites poorly use it instead of a `POST`. The more likely is a `POST` request but the complication is to append a request body. 

The attacker can design a malicious website to include an `<iframe>` hiding a form with some preset values. As soon as the JavaScript loads the page, it causes the browser to send the `POST` request and since the browser also sends the user's cookies, the request looks legitimate.

```html
<body onload="document.csrf.submit()">
 
<form action="http://example.com/transfer" method="post">
	<input type="hidden" name="amount" value="1000">
	<input type="hidden" name="account" value="Attacker">
</form>
```

The most common defense mechanism is through _anti-CSRF tokens_ which is a challenge token associated with each user and sent as a hidden value for every state-changing form in the web app:
- the web server generates and stores a token
- the token is set as a hidden field of the form
- the token is included in the `POST` request when the form is submitted
- the server compares the token sent with the stored one
- if they match, the request is valid

```html
<form action="http://example.com/transfer" method="post">
    <input type="hidden" name="CSRFToken" value="OWY4NmQwODE4ODRjN2Q2NTlhMmZlYWEwYzU1YWQwMTVhM2JmNGYxYjJiMGI4MjJjZDE1ZDZMGYwMGEwOA==">
</form>
```

There are complications on [how to generate these tokens](https://owasp.org/www-project-csrfguard/) so that they're unique per session, secret and unpredicable. Best check the [OWASP CSRF Prevention Cheat Sheet](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md).
</div>

- [ ] Use anti-CSRF tokens using a tested implementation 
- [ ] Use `SameSite: Strict/Lax` in cookies to disallow transactional pages linked from external sites
- [ ] Verify the origin of request with [forbidden headers](https://developer.mozilla.org/en-US/docs/Glossary/Forbidden_header_name) like `Origin` and `Referer`; these can't be altered
- [ ] Use a `__Host-token` prefix in a secure cookie which can't be overwritten
- [ ] Use the [Double Submit Cookie](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md#double-submit-cookie) alternative when the CSRF token can't be used

### Ensure protection against other <mark>injection attacks like XFS and CRLF</mark>.

<div class="note" markdown="1">
Cross-Frame Scripting (XFS) attacks are used in conjunction with XSS place malicious JavaScript code inside an `<iframe>` to load a legitimate page in order to steal data from unsuspecting victims. It is usually initiated through social engineering. The user is tricked into loading a page controlled by the attacker and enters credentials into the legitimate site displayed by the iframe, while the malicious script steals the keystrokes.

Read more about XFS [here](https://owasp.org/www-community/attacks/Cross_Frame_Scripting).

Carriage Return and Line Feed (CRLF) attacks occurs when malicious content is inserted into HTTP response headers after the victim clicks on a malicious link.

</div>

- [ ] Validate inputs against malicious code that can be injected into a frame
- [ ] URL-encode strings before sending or setting inside cookies
- [ ] Filter out metacharacters e.g. `,`, `;`, `CR`, `LF` from user data
- [ ] Set the `X-Frame-Options` [header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options) to avoid click-jacking attacks


### Ensure protection against <mark>sensitive data exposure</mark>.

- [ ] Correctly identified sensitive data according to regulatory requirements and business needs
- [ ] Provide team training to be able to identify sensitive data
- [ ] Provide visible guidelines on how to treat sensitive data, including responding to Customer Support queries
- [ ] Do not store or cache sensitive data unecessarily, discard it as soon as processed for reasonable needs
- [ ] Do not display sensitive information as clear text, instead use tokenisation or truncation, e.g. for masking card numbers
- [ ] Use a [federated identity management](https://www.okta.com/blog/2019/05/what-is-federation-and-why-should-your-apps-support-it/) over custom passwords, when possible
- [ ] If you need to store passwords, use cryptographic [salted hashing](https://en.wikipedia.org/wiki/Bcrypt) to protect against _rainbow table_ attacks
- [ ] Encrypt data in transit and at rest with adequate encryption strength

### Ensure that <mark>debug logs, exceptions and error messages</mark> do not leak information.

<div class="note" markdown="1">
Sensitive data, like user passwords, health records and card information should never be logged, not even encrypted because they can be decrypted by a rogue employee. Monzo had [a close call](https://monzo.com/blog/2019/08/05/weve-fixed-an-issue-storing-some-customers-pins) with user PINs written in encryted logs.
</div>

- [ ] Don't accidentaly make debugging, exceptions and unwanted error messages visible
- [ ] Clean error messages visible to the users from sensitive information
- [ ] Use `POST` requests to ensure confidential information is not visible in query string params

### Ensure <mark>cookie security</mark> with the appropriate flags.

 - [ ] Use `Secure` to ensure sensitive cookies like session IDs are only transmitted over `HTTPS`
 - [ ] Use `HttpOnly` to block access to cookies from the JavaScript client-side code
 - [ ] Use `SameSite: Strict/Lax` to [limit cookies to same-site requests](https://web.dev/samesite-cookies-explained/)


### Ensure <mark>URL redirection validation</mark> is in place.

- [ ] Use a list of approved URLs or domains allowed for redirections
- [ ] Validate input parameters using whitelisting


### Ensure protection against <mark>session fixation attacks</mark>.

- [ ] Invalidate existing session IDs before authorising a new session

### Ensure protection against <mark>directory traversal attacks</mark>.

- [ ] Use a whitelist of acceptable data inputs to prevent influencing paths and names used in file system operations

### Ensure protection against DoS attacks through <mark>improper resource release</mark>.

- [ ] Ensure any created and allocate resource e.g. connections is properly released after use


### Automate <mark>CVE checks</mark> for third-party dependencies e.g. frameworks and libraries.

<div class="note" markdown="1">

The effort to integrate a CVE-scanning tool will pay its weight in gold. This can be done directly at the code repository level, for example with [Github Alerts, Advisories and Security Policies](https://docs.github.com/en/github/managing-security-vulnerabilities/adding-a-security-policy-to-your-repository) or with an awesome third-party tool like [Snyk](https://snyk.io/product/vulnerability-database/).

</div>
