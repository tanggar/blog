---
date: "2024-11-28T22:46:56-05:00"
draft: true
title: "301 Redirects"
---

Redirects are one of the responses that a server may send back to a client upon a request for a resource. For example, let's just say that `www.test.com/abc` has been moved to `www.test.com/zzz`. If the server gets a request for /abc, it can inform the client that it has been moved by responding with a redirect response, and setting the `location` response header to `www.test.com/zzz`. But there are two different kinds of redirects, 301 permanent and 302 temporary.

A 301 permanent redirect should mean that the resource has been permanently moved to the new location, and that it will never be found again at the old one. If you are sending 301 redirects, make sure that you mean it! Browsers will cache these quite aggressively by default and just always send the user to the new location. Later on, if you remove the redirect, users who have the cached redirect response would still be redirected due to the browser caching it.

To be on the safe side, you probably want to use 302 temporary redirects unless you are certain that the old location will never be brought back. Browsers will not cache these in the same way (???). Alternatively, you can also send back a 301 redirect, but with some cache headers to limit how long the client would keep them for. This way, it would eventually expire and the user can retrieve the resource at the original URL again.

However, if the damage has been done and some users have a cached 301 redirect, is there a way to fix it? One way is to ask the user to clear their cache, which requires extra work on their part. The other fix would require you have control over the location the user was being redirected to. If you can redirect the user back to the original location, then that could solve the issue. The browser would go into a redirect loop for a bit (as it just gets bounced between the two). After a while, the browser will invalidate the cached redirect and make a request to retrieve the resource at the original location again. This should then result in the user breaking out of the loop and getting the original resource again.
