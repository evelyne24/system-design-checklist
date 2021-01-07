---
layout: default
title: Content Delivery Network
parent: System layers
permalink: /layers/cdn
nav_order: 2
---

{% include toc.md %}

## Content Delivery Network (CDN)
{: .no_toc }

### Determine if the <mark>static and dynamic content</mark> can be served closer to the user.

- [ ] Serve static content (e.g. files like pdf, videos, images)
- [ ] Serve dynamic content (e.g. weather data via an API)
- [ ] Stream real-time audio/video

### Determine if you want to serve <mark>content via HTTP2 </mark> for increased performance.

<div class="note" markdown="1">

The `HTTP/1` specification recommends opening up to two connections from a client (browser) to a web server. Modern browsers have removed this restriction and can handle more than two parallel downloads. This decreases page load time when downloading multiple assets (CSS, JavaScript files, images) for a single page view.

**Domain sharding** is an older workaround for `HTTP/1` limitation by _sharding_ the domain with multiple `CNAME` aliases for the same web server.

`HTTP/2` solves this with _multiplex responses_, by interleaving multiple responses in parallel without blocking. This allows the web server to serve multiple files at the same time in a single connection.

|        ![Http2 request and response multiplexing](https://developers.google.com/web/fundamentals/performance/http2/images/push01.svg)         |
| :-------------------------------------------------------------------------------------------------------------------------------------------: |
| [Http2 request and response multiplexing](https://developers.google.com/web/fundamentals/performance/http2#request_and_response_multiplexing) |

</div>

- [ ] Use `HTTP/1`
- [ ] Use `HTTP/2`

### Determine whether to serve content from <mark>multiple domains from a dedicated IP address</mark>.

<div class="note" markdown="1">

Sometimes you want to host multiple secure domains e.g. `https://mycompany.com` and `https://api.mycompany.com`, each with its own SSL certificate, on a single web server which has one IP address.

The difficulty is that the SSL handshake between the server and clients happens _before_ the clients can indicate the domain they want to reach using the `Host` header.

**Server Name Indication (SNI)** is an extension of the TLS protocol which solves this issue by including the domain name in the TLS handshake, so that clients are able to see the correct SSL certificate for the website they want to access.

|                                                        ![Server Name Indication](assets/img/sni.png)                                                         |
| :-----------------------------------------------------------------------------------------------------------------------------------------------------------: |
| _[Server Name Indication](https://aws.amazon.com/blogs/networking-and-content-delivery/unpacking-sni-based-ssl-and-dedicated-ip-ssl-for-amazon-cloudfront/g)_ |

</div>

- [ ] Serve multiple domains with SNI
- [ ] Serve multiple domains via dedicated IP addresses

### Determine whether to <mark>restrict content access</mark> to authenticated users only.

For example, only paid subscribers can download files.

- [ ] Restrict access with signed URLs
- [ ] Restrict access with signed cookies
- [ ] The content is available to unauthenticated users

### Determine the content update <mark>versioning</mark>.

<div class="note" markdown="1">

The recommendation is to use _different file or directory names_, for example `image_1.png`, `image_2.png` because you don't have to wait for an object to expire before your CDN servers the new version (also, invalidations cost money).

Setting _cache eviction policies_ (see below) is also recommended alongside versioning.

</div>

- [ ] Update content using the same file names
- [ ] Update content using a version identifier in file names

### Determine the <mark>caching policy</mark> to reduce latency and network traffic.

- [ ] Set `Cache-Control` headers for caching policies
- [ ] Set `ETag` or `Last-Modified` cache validator headers
- [ ] Use `Vary: User-Agent` to make platform-specific file versions discoverable by search engines
- [ ] Set the default, minimum and maximum TTL

### Check if <mark>CORS</mark> is set up correctly.

<div class="note" markdown="1">

**Cross Origin Resource Sharing (CORS)** is a mechanism built in browsers to allow requests from the same domain but block rogue requests to a different:

- _domain_: `example.com` → `api.com`
- _subdomain_: `example.com` → `api.example.com`
- _port_: `example.com` → `example.com:4000`
- _protocol_ `https://example.com` → `http://example.com`

The idea is to _prevent cross-scripting attacks_. For example, we don't want Google Ads to make an AJAX call to your logged in bank account.

CORS works in the following way:

- If the server doesn't respond with specific headers to `GET` and `POST` requests, _the request will still be sent_, _the response still received_ but _the browser won't allow the JS to read the response_.

- If the browser makes a request with _cookies_ or a _custom header_ and a `Content-Type` other than `application/x-ww-form-urlencoded`, `multipart/form-data` or `text-plain` then a mechanism called **_preflight_** is used and an `OPTIONS` request is sent to the server instead.

- If the server doesn't respond properly, _only the preflight request will happen_ and the browser will display an error:
  `Access to fetch https://api.example.com from origin https://example com has been blocked by CORS policy...`

CORS can be controlled with the following headers:

1. `Allow-Control-Allow-Origin`

   - returned by the server
   - indicates what client domains are allowed to access resources:
   - `*` - any domain (anti-pattern)
   - `https://example.com`
   - if you require authentication headers (e.g. cookies) you cannot use `*`

2. `Allow-Control-Allow-Credentials`

   - required only if the server supports authentication via cookies
   - the only valid case is `true`

3. `Allow-Control-Allow-Headers`

   - comma-separated list of request header values the server is willing to support
   - if you use custom headers like `x-authentication-token`, you need to return it in this ACA header response to the `OPTIONS` call, otherwise the request will be blocked

4. `Access-Control-Expose-Headers`

   - the server makes available a comma-separated list of headers to the client, others are restricted

5. `Access-Control-Allow-Methods`

   - comma-separated list of verbs `GET`, `POST`

6. `Origin`

   - part of the request the client is making
   - contains the domain from which the application is started
   - browsers don't allow you to overwrite this value

**Bonus**: Set the `Content-Security-Policy` directives to allow loading javascript libraries only from your domains, to mitigate the risk of malicious third-party scripts.

</div>

- [ ] Set up `CORS` headers
- [ ] Set up `CSP` headers

### Check if <mark>data compression</mark> is enabled to reduce bandwidth and latency.

<div class="note" markdown="1">

This makes website rendering faster, because files are downloaded faster. The amount of compressed data is lower than uncompressed, so it also reduces costs.

Clients must use `Accept-Encoding` to indicate support for compressed files `gzip` or `br` and the server must respond with `Content-Length` to determine the size of the file.

[Comparing Broli with Gzip](https://expeditedsecurity.com/blog/nginx-brotli/), it has support across major browsers, it's faster and produces smaller files:

- HTML files are 21% smaller
- CSS files are 17% smaller
- JS files are 14% smaller

</div>

- [ ] `Gzip` compression is enabled
- [ ] `Broli` compression is enabled

### Check if a <mark>web firewall</mark> is enabled to prevent common attacks.

<div class="note" markdown="1">

A CDN is like the front door of your house. You want to lock it, control who has access and ensure nobody can storm in.

A web firewall enables the CDN to _allow_, _block_ or _count_ requests matching certain _rules_.

These rules prevent common exploits like _cross-site scripting_ (XSS) and _SQL injection_ (SQLi) attacks and/or filters unwanted traffic, for regulatory requirements.

</div>

- [ ] Check IP addresses that requests originate from
- [ ] Check they country that requests originate from
- [ ] Check certain values in request headers
- [ ] Check regex strings that appear in requests
- [ ] Check the length of requests
- [ ] Check the presence of SQL code in requests to prevent SQLi
- [ ] Check the presence of a script in requests to prevent XSS

### Check if <mark>DDoS protection</mark> is enabled.

<div class="note" markdown="1">

A DoS, or a particular form if it, Distributed Denial of Service (DDoS) means that a single malicious source or a multitude of systems target a vulnerable network and flood it with packets.

Not only that legitimate customers cannot be served but apparently, the average cost of a DDoS is around [$40,000 per hour](https://hostingtribunal.com/blog/ddos-statistics/#gref) 🤯.

A DDoS shield is a software that inspects incoming traffic and applies a combination of traffic signatures, anomaly algorithms, and other analysis techniques to detect and stop malicious traffic in real-time.

</div>

- [ ] Enable standard DDoS protection
- [ ] Enable advanced DDoS protection (i.e. real-time notifications of suspected attacks)

### Check if you <mark>logging and monitoring</mark> is enabled.

- [ ] Enable logging
- [ ] Create alarms based on log metrics, for example for `HTTP 500` status codes
