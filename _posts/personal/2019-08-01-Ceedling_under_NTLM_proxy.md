---
layout: post
title: Install Ceedling under NTLM proxy
blogid: personal
sticky: false
published: true
tags: [Ceedling, proxy, Windows, TDD, c]
---
[Ceedling](http://www.throwtheswitch.org/ceedling) is the build system for C based projects. Ceedling combines CMock, Unity, and CException in one open source package for producing quality C code.
The first step to use Ceedling is to install it.

# Problem description

Ceedling documentation lists following command for installation (after installing Ruby):
gem install ceedling

After a lot of trial,  looks like RubyGems is not able to pick up proxy on mine corporate Win7 machine.

![407 "Proxy Authorization Required"]({{"/images/img/2019-08-01/407_error.png"|relative_url}}){: .center-image }

What have been tested:
````
gem install ceedling -p http://USERNAME:PASSWORD@PROXY_URL:PROXY_PORT
gem install ceedling --http-proxy=http://USERNAME:PASSWORD@PROXY_URL:PROXY_PORT
````
All the commands above give:
````
407 "Proxy Authorization Required"
````

Setting environment variables via command line
````
set HTTP_PROXY=http://USERNAME:PASSWORD@PROXY_URL:PROXY_PORT
set HTTP_PROXY=http://USERNAME:PASSWORD@PROXY_URL:PROXY_PORT
set HTTP_PROXY=http://USERNAME:PASSWORD@PROXY_URL:PROXY_PORT
````
results in the same error.

# Solution

[Fast and simple solution](https://stackoverflow.com/questions/4418/how-do-i-update-ruby-gems-from-behind-a-proxy-isa-ntlm/4431) is to use local proxy, e.g. Fiddler. it does not require configuration and works out of the box.:  
Install and run [Fiddler](www.fiddler2.com).
Run gem:
````
$ gem install --http-proxy http://localhost:8888 $gem_name
````
RubyGems does not work behind NTLM proxy used in many companies. Currently there is no elegant way to bypass it natively, without using local proxies.

