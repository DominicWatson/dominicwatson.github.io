---
layout: post
title: 128-bit AES Encryption with Railo
time: 2015-01-14 13:21:00 +00:00
---

Just before our penultimate demo with a client before go live, we discovered that our payment gateway had shut down their test environment for the now deprecated (news to us) protocol version we were using. This led to frantically changing the code to comply with their new protocol which meant encrypting data with the following instructions:

<blockquote>[The data] should then be encrypted using AES (block size 128 - bit) in CBC mode with PKCS#5 padding using the provided password as both the key and initialisation vector and encode the result in hex (making sure the letters are in upper case).</blockquote>

Doing this in Railo turned out to be a breeze, though I needed a little reading around and experimenting to get there.

<!--more-->

## Related writing that didn't quite get me there

I first tried googling and found a couple of useful posts that got me most of the way there:

* Marco Tomic's Java based approach: [http://www.markomedia.com.au/aes-128-padded-encryptiondecryption-with-railo-java-and-as3/](http://www.markomedia.com.au/aes-128-padded-encryptiondecryption-with-railo-java-and-as3/) (this could work but I felt there should be a native approach)
* Ben Nadel's post on using AES encryption in Railo: [http://www.bennadel.com/blog/2239-jason-dean-tells-me-to-use-aes-advanced-encryption-standard-encryption.htm](http://www.bennadel.com/blog/2239-jason-dean-tells-me-to-use-aes-advanced-encryption-standard-encryption.htm). This all worked but lacked examples of using a provided password for both key and salt.

## Getting it working as per the payment gateway spec

The key here was setting the encryption key to a Base64 encoding of the password and setting the Salt as the passphrase using an extra parameter to the `Encrypt()/Decrypt()` methods (documentation here: [http://www.railodocs.org/function/encrypt](http://www.railodocs.org/function/encrypt)):

{% highlight js %}
var encryptionKey = ToBase64( passphrase );
var salt          = passphrase;

var encrypted     = Encrypt( input, encryptionKey, "AES/CBC/PKCS5Padding", "hex", salt );
{% endhighlight %}

The decryption was exactly the same but using 'Decrypt':

{% highlight js %}
var encryptionKey = ToBase64( passphrase );
var salt          = passphrase;

var encrypted     = Decrypt( input, encryptionKey, "AES/CBC/PKCS5Padding", "hex", salt );
{% endhighlight %}

Simple. Hopefully this will be useful to someone one day.

Dominic