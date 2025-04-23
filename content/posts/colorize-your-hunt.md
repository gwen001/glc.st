---
date: 2018-08-01
title: Colorize your hunt
images: [/images/mbh.png]
tags:
- bug bounty
- burp suite
- tools
- chrome
---
As a full time bug hunter, it's important to use the tools you are confortable with, sometimes a small improvement can change your life.
During the great 3 days course presented by [Nicolas Grégoire](http://www.agarri.fr/), he showed us a browser called Autochrome.
Combined with a tiny Burp Suite extension, it becomes very easy to visualize the things you really want to see and reduce the noise for your eyes.
In this article I will show you my current configuration.
<!--more-->


## Autochrome

[Download on Github](https://github.com/nccgroup/autochrome)

One of the advantage of using this browser is that several options are already configured:  
XSS auditor is disabled  
Certificate errors are ignored  
Proxy server configured  
...

Check the file `~/.local/autochrome/chrome` to see them all and customize the configuration as you want.
I personnally added Flash support:
```none
  "--ppapi-flash-path=/usr/lib/adobe-flashplugin/libpepflashplayer.so",
  "--ppapi-flash-version=30.0.0.134"
```

Resetting cookies or other cached datas is possible with the help of a single button, as well as switching to another profile.  
![Autochrome](/images/autochrome.png)

The main point is that profiles DO NOT share datas so you can connect to the same website with different users.
This is absolutely awesome when you need to test several levels of permissions. For instance: unauthenticated user, connected user, administrator...
By default, only three profiles are available: red, yellow, blue.
You have to update the script `lib/auto_chrome/profile_builder.rb` in order to enable them all, line 23:
```ruby
if @profile_names.empty?
  @profile_names = %w(Red Yellow Blue Green Orange Purple Cyan White)
end
```

Then generate the profiles:
```sh
ruby autochrome.rb -P
```

White seems to be buggy so seven themes are now available.
Since yellow is my favorite color, I choosed to create an alias:
```sh
alias ac='~/.local/autochrome/chrome --profile-directory=Yellow 2>/dev/null &'
```


## Multi Browser Highlighting

Download on [BApp Store](https://portswigger.net/bappstore) or [PortSwigger Github](https://github.com/PortSwigger/multi-browser-highlighting)

*"A simple burp plugin that highlights the Proxy history to differentiate requests made by different browsers. The way this works is that each browser would be assigned one color and the highlights happen automatically."*

Basically this extension will link a Burp Suite color to a User-Agent and then highlight the requests in the proxy tab according to it.

I altered a little bit the code in order to deal with Autochrome.
I also added a small test to "negate" the out of scope urls. They will now be appear in grey, because I always associated grey as the "disable" status so now my eyes don't catch this color.

![Autochrome](/images/mbh.png)

<span style="background-color:#FFFFFF;">White: user 1</span>  
<span style="background-color:#FFFF64;">Yellow: user 2</span>  
<span style="background-color:#64FFFF;">Cyan: manager 1</span>  
<span style="background-color:#64FF64;">Green: manager 2</span>  
<span style="background-color:#FF6464;">Red: administrator</span>  
<span style="background-color:#B4B4B4;">Grey: out of scope</span> (related to scope tab)  


## Logger++

Download on [BApp Store](https://portswigger.net/bappstore) or [PortSwigger Github](https://github.com/PortSwigger/logger-plus-plus)

*"Sometimes it is necessary to log all the requests and responses of a specific tool in Burp Suite. Logger++ can log activities of all the tools in Burp Suite to show them in a sortable table. It also has an ability to save this data in CSV format."*

What more can I say ? In a nutshell this extension logs absolutly ALL requests, that's it.
You can apply filters to hide requests or regexp search in your traffic history, this is very useful.

I added filters to fit Autochrome's colors, that way it's looks like the Proxy tab.
![autochrome android green.](/images/logger++-filters-ac.png)


## Issues

The main problem is that Multi Browser Highlighting is not compatible with Collaborator Everywhere which also alter the User-Agent to inject a custom payload.  
**Edit:** solution from [James Kettle](https://twitter.com/albinowax): swap the order the extensions are executed in using the Up/Down buttons on the extender tab.

"Purple" doesn't exist in Burp Suite (even if blue looks like purple it's not) so you have to apply another color, like magenta or pink.

I didn't find an option to save color filters in Logger++, seems to be auto but I would like to save my entries in a config file and be able to reload them in case of emergency.

If you find a way to fix any of this, please let me know!


## Conclusion

This configuration comes very handy when you need to test a platform where users can have several permission levels, test IDOR, test vertical/horizontal escalation.

It could also be used to differentiate different types of browser, let's say that you want to test a website with 
<span style="background-color:#FFFF64;">Chrome</span>, <span style="background-color:#FFC864">Firefox</span>, 
<span style="background-color:#FF6464;">Opera</span>, <span style="background-color:#64FFFF;">IE</span> and 
<span style="background-color:#64FF64;">Android</span> at the same time.
You simply have to add a quick rule in your proxy options.  
![Autochrome](/images/ac-android-green.png)

Finally it's a nice feature to use when you create your PoC for your reports, the company will enjoy how easy it is to read your screenshots/videos and differenciate the requests of a user/browser from another one.

