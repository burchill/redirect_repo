# redirect_repo
## A cheap way of redirecting to a local IP

I have a RaspberryPi Flask website running on my local network.  Instead of remembering the Pi's IP address or configuring my DNS, etc., I have come up with this system, which redirects a public URL to my private local site.  I do this via the following.

First, I have a domain name that refers to my GitHub Pages site (zachburchill.ml --> burchill.github.io), using the normal CNAME method.
Then, I added an additional CNAME record to the registrar's account, for the subdomain `scale.zachburchill.ml` (named that because my local site is built around tracking my weight). 

Now my registrar's record table looks like this:

| Name | Type | TTL | Target |
--------|-------|------|----------|
| | A | 14440| 192.30.252.153 |
| | A | 14440| 192.30.252.154 |
| WWW | CNAME | 14440 | burchill.github.io |
| SCALE | CNAME | 3600 | burchill.github.io |

Then I added the `CNAME` file to this directory and included the `index.html` file that automatically redirects the browswer to the local site.  

This worked well, but I realized I couldn't do something like `http://scale.zachburchill.ml/weight_tracker/` and have it redirect to `http://<local_ip>/weight_tracker/` since the `weight_tracker` page doesn't exist as a page in this public repo.  I came up with a clever (in my opinion) workaround: since non-existent page requests are (generally) redirected to the `/404.html/` page, I could make a 404 page that would get the URL the user was *attempting* to visit and redirect them to that page on the local site (via Javascript). 

With that, I can type `scale.zachburchill.ml/weight_tracker/` and have it directed to `http://<local_ip>/weight_tracker/` automatically.
