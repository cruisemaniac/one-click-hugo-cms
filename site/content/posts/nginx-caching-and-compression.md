---
comments: true
date: "2013-12-07T11:00:00Z"
description: How to enable caching and compression on Nginx for better performance
tags:
- tech
- nginx
- ubuntu
- caching
- compression
- performance
title: Nginx HTTP Caching and Compression
---
This is a blog post for posterity - I'm sure I'll forget how I did this and will go hunting again! and I must definitely be the last guy on the planet to be setting up nginx!

Nginx is pretty fast in itself. I use [jekyll][1] to power this site. So, when you're reading this article, all the data is coming right off the hard disk - No databases, no queries, no lookups - simple fopen, read, stdout…

But you still want a bit more… A little bit of added juice from nginx. This is what I did - enabled http Caching and gZip compression.

And here's how:

####nginx.conf

Open up your `nginx.conf` file (on my box, it was at `/etc/nginx/nginx.conf`) and head to the section under Gzip settings.

Uncomment the following chunk:

{{< highlight nginx >}}

        ##
        # Gzip Settings
        ##

        gzip on;
        gzip_disable "MSIE [1-6]\.(?!.*SV1)";

        gzip_vary on;
        gzip_proxied any;
        gzip_comp_level 6;
        gzip_buffers 16 8k;
        gzip_http_version 1.1;
        gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;

{{< / highlight >}}

What we're doing here is this:

1. Turning on gzip compression - this must be on by default on nginx.
2. Disabling compression for browsers that do not support compression
3. `gzip_vary` helps set headers with `vary: Accept-Encoding` and `gzip_proxied` allows compression to happen even if the requests are coming in via a web proxy.
4. `gzip_comp_level 6` defines the level of compression between 1-9 (1 being lowest and 9 being highest)
5. And other settings - the types of mimes to be pushed through the compression, the keep alive settings for the http 1.1 protocol, et al. 

What really matters here is that you uncomment these lines.

Save the file and restart nginx.

####sites-enabled

To enable HTTP Caching, head out into `/etc/nginx/sites-enabled/<your-site-config-file>` and make the following changes:

{{< highlight nginx >}}

    #enable caching
        location ~* \.html$ {
                expires -1;
        }

        location ~* \.(css|js|gif|jpe?g|png)$ {
                expires 15d;
                add_header Pragma public;
                add_header Cache-Control "public, must-revalidate, proxy-revalidate";
        }
{{< / highlight >}}

The first chunk disables expiry for html files. This will cause the text matter to be downloaded every time.

The second chunk sets the expiry to 15 days for all other content of the website - css files, javascript and images - I've set it to 15 days because there's not much changes these files will see.

Another restart of nginx and you must notice a decent improvement in performance.

A quick way to check if your settings are in place is by using `curl`.

{{< highlight console >}}

the-flying-dutchman:cruisemaniac.com cruisemaniac$ curl -I cruisemaniac.com/favicon.png
HTTP/1.1 200 OK
Server: nginx/1.1.19
Date: Sat, 07 Dec 2013 05:26:48 GMT
Content-Type: image/png
Content-Length: 34402
Last-Modified: Fri, 06 Dec 2013 23:57:03 GMT
Connection: keep-alive
Expires: Sun, 22 Dec 2013 05:26:48 GMT
Cache-Control: max-age=1296000
Pragma: public
Cache-Control: public, must-revalidate, proxy-revalidate
Accept-Ranges: bytes

{{< / highlight >}}


[1]: http://jekyllrb.com/ "Jekyll"