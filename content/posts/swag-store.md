---
date: 2019-07-13
title: Swag Store
images: [/images/swag-store.png]
tags:
- bug bounty
- swag
- project
see_also:
  -
    link: Shopify REST Admin API reference
    url: https://help.shopify.com/en/api/reference
---

In the bug bounty industry, companies/programs reward hackers with bounties for their contribution, most of the time money and swag from time to time.
But what about platforms?
How platforms thank their hackers?
Unfortunately events are not a standard and only a few selected hackers can participate.
What about something more popular, something fair, something that all hunters could enjoy, even the beginners, even the casuals?
All platforms have a scoring system so what if you could transform this reputation/score to something more concrete?
This is THE SWAG STORE.
<!--more-->

For the purpose of this article, I created a demo store located at:
[https://bbswagstore.myshopify.com/](https://bbswagstore.myshopify.com/)  
_<u>Please do not focus on prices!</u>_


## Concept

Basically the idea of the Swag Store is to convert reputation points into something more interesting for hackers than a number on a webpage: __get real items__.
Some of those items are more a quest treasure like the famous Hackerone hoodie, not easy to get if you're not part of an event.
I would be more than happy to be able to buy one, or the amazing Bugcrowd stuff or the Yes We Hack backpack but I'm not starving enough to hunt days and night for months/years.
How many bugs should I find for that?


Plus, as a hunter I think we deserve it, <u>we are part of their success</u>, no hackers no platforms.
Since most of the platforms get a part of hunters rewards - usually 25% - a hacker who reaches 1000 points had probably reported enough bugs to pay much more than a tee shirt.
But it's only my opinion.

![swag store bugcrowd](/images/swag-store-bugcrowd.png)


## Get better

When we talk about swag we usually think about tee-shirt, stickers, caps, camera covers and so on...
But wait... What if we could get something more __useful for everyone__?
Maybe we could find a way to improve hackers skills at the same time?
It would be a win/win because if hackers get better, they would find more bugs right?

Think if, as a hacker, you could get that famous __OSCP subscription for free__.
What you have to do is to find some bugs on some targets (and also be payed for that!).
Wouldn't it be a good deal?

Think if, as a platform you could tell your customers that __80% of your hunters are OSCP certified__?
Wouldn't it be a nice marketing move?

Think if, as a company you could have access to a __community of hackers who are all OSCP certified__?
Wouldn't it be a sign of quality?

![swag store learning](/images/swag-store-learning.png)


## Project details

For a quick deployment, I decided to create a store on the e-commerce solution [Shopify](https://www.shopify.com/).
Easy to configure, easy to maintain, easy products/customers/orders managment, operational in a minute, Shopify is the best option in this situation (they also have a great bug bounty program on Hackerone).
So no development needed for the store itself but we have a problem regarding the payment: how to buy items with platforms reputation?

We can easily assume that a standard tee-shirt with the platform logo would cost much more than 10Ɍ (Hackerone delivers 100Ɍ at subscription).
That means that prices have to be high, let's say 500Ɍ for that tee-shirt. That way we also avoid real orders, who would buy a tee-shirt for 500$?!

Hackers should have a **swag credit** that would move up (hopefully not down) following their reputation score.
Then the idea is to create a discount code (voucher) equivalent to the swag credit when a hacker wants to buy something.
The hacker uses the code during the payment process, the platform is notified and his swag credit decreases regarding the <u>real amount of the order</u>.

Required:

- The hacker should first create an account on the swag store.
- The hacker should provide his account id from the swag store on the bug bounty platform.
- The platform should add a button in the hackers profile to create a voucher regarding the swag credit value.
- The platform should create the gateway scripts with the swag store.

![swag store profile](/images/swag-store-profile.png)


## Store configuration

- First some **basics settings regarding customers**: `/admin/settings/checkout`  
&#45; check the option _"Accounts are required"_  
&#45; check the option _"Customers can only check out using email"_  

- **Currency display**: `/admin/settings/general`  
&#45; Store currency: `US`  
&#45; HTML with currency: `Ɍ{``{amount}} REP`  
&#45; HTML without currency: `Ɍ{``{amount}}`  
&#45; Email with currency: `Ɍ{``{amount}} REP`  
&#45; Email without currency: `Ɍ{``{amount}}`  

- **Disable payments**: `/admin/settings/payments`  
&#45; Disable PayPal Express Checkout  
&#45; Create a "Manual payment method" with dummy datas  

- **Create a private app** in order to talk with the store using Shopify API: `/admin/apps/private/new`  
Give `Read and write` permission to:  
&#45; _Customer details and customer groups_  
&#45; _Discounts - Discounts GraphQL API_  
&#45; _Discounts - PriceRule REST and GraphQL API_  

- Finally, **configure the webhook** to be notified of the payments: `/admin/settings/notifications`  
&#45; event: _Order payment_  


## Scenario

**1/** The hacker creates an account on the store

**2/** He submits his store ID in his profile on the bug bounty platform

For convenience, this process could be fully automated by the platform using the [Shopify API](https://help.shopify.com/en/api/reference/customers/customer#create-2019-07).
```none
# create customer
curl https://<API_KEY>:<PASSWORD>@<STORE_NAME>.myshopify.com/admin/api/2019-04/customers.json -H "Content-Type: application/json" -X POST -d '{"customer":{"email":"g@10degres.net","accepts_marketing":false,"first_name":"Gwen","last_name":"Gwen","currency":"USD","addresses":[{"first_name":"Gwen","last_name":"Gwen","company":"10degres","address1":"my address","address2":"my address 2","city":"Paris","province":"","country":"France","zip":"75001","phone":"","name":"Gwen Gwen","province_code":null,"country_code":"FR","country_name":"France","default":true}]}}'
```

**3/** The hacker visits his profil page and decide to _"Create a discount code"_

**4/** The plaform creates the voucher on the swag store:
```none
# create price rule
curl https://<API_KEY>:<PASSWORD>@<STORE_NAME>.myshopify.com/admin/api/2019-04/price_rules.json -H "Content-Type: application/json" -X POST -d '{"price_rule": {"title": "ABCDEFGHIJKLMNOPQRSTUVWXYZ","target_type": "line_item","target_selection": "all","allocation_method": "across","value_type": "fixed_amount","value": "-10.0","customer_selection": "prerequisite","prerequisite_customer_ids":["<USER_ID>"],"starts_at": "2019-01-01T00:00:00Z","ends_at": "2019-01-02T00:00:00Z"}}'

# create discount code
curl https://<API_KEY>:<PASSWORD>@<STORE_NAME>.myshopify.com/admin/api/2019-04/price_rules/<PRICE_RULE_ID>/discount_codes.json -H "Content-Type: application/json" -X POST -d '{"discount_code":{"code":"ABCDEFGHIJKLMNOPQRSTUVWXYZ"}}'
```

For security reason:  
&#45; Price rules should be configured with the correct user id: `customer_selection + prerequisite_customer_ids`  
&#45; The end date of the validity of the "offer" should be setted to a short period: `ends_at`  
&#45; The price rule could be only used once: `once_per_customer`  

**5/** The discount code is displayed to the hacker

**6/** The hacker can now chill out on the store and pick up what he really really wants <img src="/images/icon-music.png" width="20">

**7/** During the payment process the hacker provides the discount code

If the discount is >= than the subtotal, then the final price will be 0, everything is fine  
If the discount is < than the subtotal, the hacker will have to remove some items from his cart (or pay the difference with real money but no one wants that...)

![swag store checkout](/images/swag-store-checkout.png)

**8/** The platform is notified by the store that a payment just occured


```http
POST / HTTP/1.1
Content-Type: application/json
X-Shopify-Topic: orders/paid
X-Shopify-Shop-Domain: bbswagstore.myshopify.com
X-Shopify-Order-Id: 8209946494603
X-Shopify-Test: true
X-Shopify-Hmac-Sha256: N2nqX1/zcS4953w9/et+UDvEmMy1QhNEe14zG7afjTc=
X-Shopify-Api-Version: 2019-07
Accept-Encoding: gzip;q=1.0,deflate;q=0.6,identity;q=0.3
Accept: */*
User-Agent: Ruby
Content-Length: 6441
Connection: close
Host: www.mysuperbugbountyplatform.com

{"id":8209946494603,"email":"jon@doe.ca","closed_at":null,"created_at":"2019-07-15T16:39:44+02:00"...
```

**9/** The platform decreases the swag credit of the hacker by the <u>real amount of the order</u>

**10/** (optional) The platform revokes the voucher
```sh
# delete price rule
curl https://<API_KEY>:<PASSWORD>@<STORE_NAME>.myshopify.com/admin/api/2019-04/price_rules/<PRICE_RULE_ID>.json -H "Content-Type: application/json" -X DELETE

# delete discount code
curl https://<API_KEY>:<PASSWORD>@<STORE_NAME>.myshopify.com/admin/api/2019-04/price_rules/<PRICE_RULE_ID>/discount_codes/<DISCOUNT_CODE_ID>.json -H "Content-Type: application/json" -X DELETE
```

**11/** (optional) Using the same method, the platform can revokes all voucher created the day before but not used


## Conclusion

My store is just a small example of what could be possible, we can imagine everything: classic goodies, books, courses, **conventions invites** (most of the time very expensive), software licenses, IOT stuff... etc...

In an improved version of a Swag Store we can imagine that programs items could also be available: Pornhub, Etsy, GitHub, Eset, they all have incredible swag very popular in the community.

Why companies triagers couldn't enjoy swag too?
We could imagine that the reputation meters of the programs increase the same way than hackers meters.
That way a company could "buy" great items (like OSCP) to their triagers.
Again it would be a great marketing move.
And maybe some triagers will think twice before mass closing reports as NA/dup.

<!--
trading system

Let's says that a hacker already got several swag pack from his favorite platform? Let's say that he already got that Hackerone cap 5 times, another hacker doesn't have it but he has 5 tee-shirt. Maybe they could do a trade?
-->


## External resources

- [Shopify REST Admin API reference](https://help.shopify.com/en/api/reference)
