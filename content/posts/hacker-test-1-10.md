---
date: 2015-03-05
title: Hacker Test 1-10
tags:
- training
---
[Hacker Test](http://www.hackertest.net/ "Hacker Test") is an online hacker simulation.
20 levels to test your PHP, HTML and Javascript knowledge.
Below the solution of the first ten.

> Hackers solve problems and build things, and they believe in freedom and voluntary mutual help. 
To be accepted as a hacker, you have to behave as though you have this kind of attitude yourself. 
And to behave as though you have the attitude, you have to really believe the attitude.

## Level 1

In the source code we can see the following JavaScript lines:

```javascript
var a="null";

function check()
{
  if (document.a.c.value == a) {
    document.location.href="http://www.hackertest.net/"+document.a.c.value+".htm";
  } else {
    alert ("Try again");
  }
}
```

The value of the password input named `c` is compared with the variable `a`, which has a `null` value. This is the password: `null`

<!--more-->

## Level 2

After submitting the first level, a prompt immediatly ask for a password, this is the second level. 
If you provide a wrong value or press escape, you will access to the page and obviously her source code:

```javascript
var pass, i;

pass=prompt("Please enter password!","");

if (pass=="l3l") {
  window.location.href="http://www.hackertest.net/"+pass+".htm";
  i=4;
}
```

The password appears in plain text: `l3l`

## Level 3

You can use the same technique as before but the password seems to have been hidden:

```javascript
var pw, Eingabe;

pw=window.document.alinkColor;
Eingabe=prompt ("Please enter password");

if (Eingabe==pw) {
  window.location.href=String.fromCharCode(97,98,114,97,101)+".htm";
} else {
  alert("Try again");
}
```

There is two solutions to pass this level.

- Provide the good password: you have to evaluate the expression `window.document.alinkColor`, 
look at the `<body>` tag it contains the info or simply use the console of your favorite browser, result is: `#000000`
- Bypass the level: in case of a success result, you will be redirected to an url which is char encoded (`97,98,114,97,101`), 
simply decode it and you will be able to access the next level located at: `abrae.htm`

## Level 4

Click on the link to pop the password prompt. Spam the escape key and it's seems to be enought ?! No password required here...

## Level 5

No challenge here, same as level 2, provide the clear password in the source: `SAvE-as hELpS a lOt`

```javascript
var pass, i;
pass=prompt("Password: ","");
if (pass=="SAvE-as hELpS a lOt") {
  window.location.href="save_as.htm";
  i=4;
} else {
  alert("Try again");
  window.location.href="abrae.htm";
}
```

Or directly call the url of the next level: `save_as.htm`

## Level 6

No plain text or encrypted password in the HTML source but a JavaScript seems to be included:

`<h1>Level 6</h1><SCRIPT SRC="psswd.js" LANGUAGE="JavaScript" type="text/javascript"></script>`

As the famous king of pop would say, "this is it":

```javascript
var pass;
pass=prompt("Password:","");
if (pass=="hackertestz") {
  window.location="included.htm";
} else
alert("Try again...");
```

Plain text password is not a good security: `hackertestz`

## Level 7

When providing empty or wrong credentials you'ill be redirected to `pwd2.php` and faced to this message: `Authentication Failed. Try again.`
There is nothing interesting in this page.

Back to the forms one, an image is used: `images/included.gif`. 
Take a look at the bottom right of this big picture and get the credentials:

![hackertest level7](/images/hackertest-level7.png)

## Level 8

There is no info inside the original page but after submitting the form with wrong credentials you’ll be redirected to `phat.php` and faced to this message: `Authentication Failed. Try again.`

Again, in the source an image is used `images/phat.gif`, at the bottom right of this picture a message is displayed: `Look for a .PhotoShopDocument!`

As the uppercase letters say the extension of a Photoshop document is `.psd` so change the url to `images/phat.psd`. Your browser will merely download the file. I used GIMP to open it instead of Photoshop but it doesn't matter.

![hackertest level8](/images/hackertest-level8.png)

## Level 9

The trap in this level is to think that the document is empty. 
But if you look carefully the source you'll notice that there is about 1500 lines of code.
Scroll down to find the clue.

```html
<!-------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------
--------------------------------------------  Password: Z2F6ZWJydWg= add a page 
extention to that  --------------------------------------------------------------------
---------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------
-------------------------------------------------------------------->
```

The base64 password can be easily decoded with online resources. 
After trying different file extensions you would find the good one: `gazebruh.php`

## Level 10

This level is  a little bit tricky, it’s all about observation. 
The note about cookies and the form itself are just diversion. 
The clue is in the top text, read carefully the intro:

> **S**treet Korner is your own online **hack**er simulation. 
W**it**h over 100 levels that require different skills to get to another step of the game, this new real-life immitation will **h**elp you advance your security knowledge. 
This site will help you improve your JavaScript, PHP, HTML and graphic thinking in **a** fun way that will entertain any visitor! 
Have a spare minute? Log on! Each level wil**l** provide you with a new, harder clue to find a way to get to another level. 
Only **f**ew people have gotten to the end of the maze... Will you crack this site?

Did you notice the bold letters? That’s the password. 
Concatenate them all and lowercase the result, you’ll find: `shackithalf`
