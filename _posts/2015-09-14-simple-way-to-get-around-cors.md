---
layout: post
title: Simple Way to get Around CORS
---

### The problem

When developing for the web, you've probably seen this before when you interact with external api's.

![My helpful screenshot]({{ site.url }}/assets/2015/09/14/cors-error.png)

For security reasons, ajax will not allow cross site scripting by default.


### Getting around CORS

There are a lot of ways to get around this, but for today, I'd like to go over a simple way to do it through docker and nginx. What we are going to build is a simple container that will proxy requests to the desired api using nginx's wonderful [proxy_pass](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass) feature.

Assuming you have docker installed, first lets go an grab nginx.

{% highlight bash %}
$ docker pull nginx:latest
{% endhighlight %}

Next lets setup the nginx config locally. Create a file named `conf/api.conf`, it should look like this.

{% highlight bash %}
# conf/api.conf

server {
    listen       80;
    server_name  default;

    location /your_proxy_path {
      proxy_pass https://apitoproxy.com;
      add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
      add_header 'Access-Control-Allow-Origin' '*';
    }
}
{% endhighlight %}

Next run docker and attach the correct volume

{% highlight bash %}
$ docker run -d -p 80:80 -v $(pwd)/conf:/etc/nginx/conf.d:ro nginx
{% endhighlight %}

Verify it works

![My helpful screenshot]({{ site.url }}/assets/2015/09/14/cors-working.png)

