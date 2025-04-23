---
date: 2019-07-31
title: Cloudflare origin server
images: [/images/cloudflare-blocked.png]
tags:
- cloudflare
- recon
- WAF
- firewall
- tools
see_also:
  -
    link: Introducing CFire - Evading CloudFlare Security Protections
    url: https://rhinosecuritylabs.com/cloud-security/cloudflare-bypassing-cloud-security/
  -
    link: Cloudflare Bypass Security
    url: http://www.securityidiots.com/Web-Pentest/Information-Gathering/Cloudflare-Bypass/Part-2-Cloudflare-Security-Bypass.html
  -
    link: Exposing Server IPs Behind CloudFlare
    url: http://www.chokepoint.net/2017/10/exposing-server-ips-behind-cloudflare.html
  -
    link: CloudFlair - Bypassing Cloudflare using Internet-wide scan data
    url: https://blog.christophetd.fr/bypassing-cloudflare-using-internet-wide-scan-data/
  -
    link: Cloudflare IP ranges
    url: https://www.cloudflare.com/ips/
  -
    link: One tweet about it
    url: https://twitter.com/gwendallecoguic/status/1043484797799223297
  -
    link: And another tweet about it
    url: https://twitter.com/gwendallecoguic/status/1113777240876228609
---

What is [Cloudflare](https://www.cloudflare.com/)? In short: Content Delivery Network (CDN), Web Application Firewall (WAF) and cherry/icing on the cake, 1 year go Cloudflare released a fast DNS resolver.
With 4 pricings and more than [16M Internet properties](https://www.cloudflare.com/case-studies/), Cloudflare is now one of the most popular firewall used for web applications.
Working as a reverse proxy the WAF does not only offer a protection against DDOS but can also trigger an alert/error when he detects an attack.
But what if you can bypass all these protections in a second making the defense useless?
<!--more-->

&gt;&gt;&gt; [Read more on Detectify](https://blog.detectify.com/2019/07/31/bypassing-cloudflare-waf-with-the-origin-server-ip-address/) &lt;&lt;&lt;

At the same time I was redacting this article, I wrote a Python script to automate some tests.

```none
usage: cloudflare-origin-ip.py [-h] [-u URL] [-s SOURCE]

optional arguments:
  -h, --help            show this help message and exit
  -u URL, --url URL     url to test
  -s SOURCE, --source SOURCE
                        datas sources separated by coma, can be:
                        censys,crtsh,local file

Examples:
cloudflare-origin-ip.py -u https://xxx.xxxxxxxxxxxx.xxx
cloudflare-origin-ip.py -u https://xxx.xxxxxxxxxxxx.xxx -s censys,crtsh (default)
cloudflare-origin-ip.py -u https://xxx.xxxxxxxxxxxx.xxx -s /home/local/ips.txt
cloudflare-origin-ip.py -u https://xxx.xxxxxxxxxxxx.xxx -s censys,crtsh,/home/local/ips.txt,/home/local/subdomains.txt

Note that this is an automated tool, manual check is still required.
```

Basically the script compares some datas (HTML, headers, Content-Type...) of the host you provide with the HTTP response of an IPs list but using the host you provide as the header `Host`.
IP sources can be: Censys, crt.sh or local files containing IPs and/or subdomains.

![cloudflare origin ip python](/images/cloudflare-origin-ip-python.png)

[Give it a try!](https://github.com/gwen001/pentest-tools/blob/master/cloudflare-origin-ip.py)


## External resources

- [Introducing CFire - Evading CloudFlare Security Protections](https://rhinosecuritylabs.com/cloud-security/cloudflare-bypassing-cloud-security/)
- [Cloudflare Bypass Security](http://www.securityidiots.com/Web-Pentest/Information-Gathering/Cloudflare-Bypass/Part-2-Cloudflare-Security-Bypass.html)
- [Exposing Server IPs Behind CloudFlare](http://www.chokepoint.net/2017/10/exposing-server-ips-behind-cloudflare.html)
- [CloudFlair - Bypassing Cloudflare using Internet-wide scan data](https://blog.christophetd.fr/bypassing-cloudflare-using-internet-wide-scan-data/)
- [Cloudflare IP ranges](https://www.cloudflare.com/ips/)
- [One tweet about it](https://twitter.com/gwendallecoguic/status/1043484797799223297)
- [And another tweet about it](https://twitter.com/gwendallecoguic/status/1113777240876228609)
