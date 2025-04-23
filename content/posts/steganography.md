---
date: 2015-10-18
title: Steganography
tags:
- stegano
see_also:
  -
    link: A very nice Steganography project from Kazuyuki Hayashi written in PHP
    url: https://github.com/kzykhys/Steganography
  -
    link: OpenStego for Linux and Windows
    url: http://www.openstego.com/
---
## Definition

Steganography is the art of hidding a message in another common message. 
The hidden message can be clear text or encrypted and the container can be anything: image, music, single text or whatever... 
The main benefit of steganography is that you can get the final message only if you know the technic used to hide it, because you'll need to use the same algorithm. 
As a second layer of protection, you can also encrypt the data with any algorithm you like or with a private key.

<!--more-->

## Example

A good old example of steganography is the famous [letter of Georges Sand to Alfred de Musset](http://5ko.free.fr/fr/sand.html)(FR). 
The original text looks like a lovely letter but you only read even lines, a porn text appears. 
If you don't know that you won't be able to see how Georges Sand was horny :)

## Least Significant Bit

Our days, with new technologies, the container can be a media support: image (jpg, gif, bmp...), video (avi, mov, flv...), sound (wav, ogg...). 
Let's see how it works with a basic technic, called Least Significant Bit aka LSB, on a png image. 
This is probably one of the easiest technic to hide a message inside an image using steganogaphy.

Each pixel of an image is made of 3 components: Red, Green, Blue. Each component can have a value from 0 to 255, so the total available colors is about `256*256*256=16777216`. 
Obviously the human eye cannot see all of those shades. 
Each RGB component is encoded on 8 bits (2^8), that means we can use the "least significant bit" aka 2^0=1 as a flag to hide any characters. 
Who will notice the difference between a full black pixel with R=0, G=0, B=0 and another pixel with R=1, G=1, B=1 ? No one for sure...

![lsb black 000000](/images/lsb-black-000000.png)
![lsb black 010101](/images/lsb-black-010101.png)

Do you see the difference ? Of course not. Do you get the point now ?

## Real case

Based on this technic, I wrote a little PHP script to put a secret message inside a PNG picture then I wrote another script for extraction:

![lsb mountain original](/images/lsb-mountain-empty.png)
![lsb mountain with message](/images/lsb-mountain-message.png)

The picture with the encoded message is the left one, obviously you cannot see the difference without an image editor, here is the extracted data:

```none
UXVlIGRldmllbnQgbGEgdmVydHUgcGVuZGFudCBjZXMgZMOpbGljaWV1eCB2b3lhZ2VzIG91IGxhIHBlbnPDqWUgZnJhbmNoaXQgdG91cyBsZXMgb2JzdGFjbGVzID8K
```

After a base64 decode, a text from Honoré De Balzac, a famous french writer, appears:

`Que devient la vertu pendant ces délicieux voyages ou la pensée franchit tous les obstacles ?`

In this test I only used the red component to store my message, that means I can only encode one bit of one character in one pixel. 
Since a character is encoded on 8 bits, the string max length is:
`(img_width*img_height) / 8`

Of course everything is possible to raise the max length, you can use the green and the blue components, you also use the second least significant bit and third, 
compress your data or you can simply use a bigger picture :) The only thing you have to matter is that the more text you'll add, the more it will alter the original picture.

## For fun

I improved my script to support every filetype in input. 
In the following picture I decide to hide a mp3 sound, there is no way to figure out the tricks:

<img src="/images/lsb-goku-empty.png" alt="lsb goku original" height="400" />
<img src="/images/lsb-goku-message.png" alt="lsb goku with audio" height="400" hspace="50" />

<audio src="/images/lsb-dbz.mp3" controls="controls"></audio>

In this test I used the three color components because of the size of the audio which was about 300Ko, the original picture was about 1.2Mo. 
Finally, after the merge, the final picture is logically, about 1.5Mo.  

[Check the script on GitHub](https://github.com/gwen001/php-stegano-lsb)


## External resources

- [A very nice Steganography project from Kazuyuki Hayashi written in PHP](https://github.com/kzykhys/Steganography)
- [OpenStego for Linux and Windows](http://www.openstego.com/)
