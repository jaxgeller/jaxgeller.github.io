---
layout: post
title: Dynamically Embed Gists with JS
---

# Dynamically Embed Gists with JS

I use github gists a good amount, they are great for storing quick snippets of things you use a lot. For example, I set up S3 static websites quite a bit, but find it hard to remember all of the different policies like so

{% highlight javascript %}
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AddPerm",
      "Effect": "Allow",
      "Principal": "*",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::example.com/*"
      ]
    }
  ]
}
{% endhighlight %}

If you can remember something like this, good on you, but for the rest of us, gists are awesome.

I was recently making some updates to my personal site, and thought it would be cool to add a part to the site that would dynamically load in all of my gists. I find gist.github.com's ui to be a little clunky and counterintuitive.

After looking at the github api for gists, it looked pretty simple. So I spun up something like this to pull in all of the gists.


{% highlight javascript %}
var URL = "https://api.github.com/users/jaxgeller/gists";
var container = document.querySelector('.post-content');

function setErr() { container.textContent = "Error fetching snippets"; }

function setGists(res) {
  var data = JSON.parse(res);
  var c = document.createDocumentFragment();
  data.forEach(function(item) {
    var d = document.createElement('div');
    d.classList.add('snippet');

    var h = document.createElement('h3');
    h.textContent = item.description;

    var s = document.createElement('script');
    s.type = "text/javascript";
    s.src = 'https://gist.github.com/jaxgeller/' + item.id + '.js';

    d.appendChild(h);
    d.appendChild(s);
    c.appendChild(d);
  });
  container.appendChild(c);
}

var request = new XMLHttpRequest();
request.open('GET', URL, true);

request.onload = function() {
  if (this.status >= 200 && this.status < 400)
    setGists(this.response);
  else
    setErr();
};

request.send();
{% endhighlight %}

Upon running that code in my jekyll blog, I encountered this `document.write` error.

`"Failed to execute 'write' on 'Document': It isn't possible to write into a document from an asynchronously-loaded external script unless it is explicitly opened. gist"`

After looking at the contents of the script, I could see that the gist embed is trying to execute 2 `document.write`; a stylesheet, and the actual gist contents.

Heres a sample of what the script is trying to do

{% highlight javascript %}
document.write('<link rel="stylesheet" href="https://assets-cdn.github.com/assets/gist/embed-3313cb70789df61d3bb75cd878325a1266ccab50b6aac9b80f1691ae0694a216.css">')
document.write('<div id=\"gist20732998\" class=\"gist\">\n    <div class=\"gist-file\">\n      <div class=\"gist-data\">\n        <div class=\"js-gist-file-update-container js-task-list-container file-box\">\n  <div id=\"file-gistfile1-txt\" class=\"file\">\n    \n\n  <div class=\"blob-wrapper data type-text\">\n      <table class=\"highlight tab-size js-file-line-container\" data-tab-size=\"8\">\n      <tr>\n        <td id=\"file-gistfile1-txt-L1\" class=\"blob-num js-line-number\" data-line-number=\"1\"><\/td>\n        <td id=\"file-gistfile1-txt-LC1\" class=\"blob-code blob-code-inner js-file-line\">server {<\/td>\n      <\/tr>\n      <tr>\n        <td id=\"file-gistfile1-txt-L2\" class=\"blob-num js-line-number\" data-line-number=\"2\"><\/td>\n        <td id=\"file-gistfile1-txt-LC2\" class=\"blob-code blob-code-inner js-file-line\">        location /proxied_site_get_around/ {<\/td>\n      <\/tr>\n      <tr>\n        <td id=\"file-gistfile1-txt-L3\" class=\"blob-num js-line-number\" data-line-number=\"3\"><\/td>\n        <td id=\"file-gistfile1-txt-LC3\" class=\"blob-code blob-code-inner js-file-line\">                proxy_pass http://www.proxied_site.com/;<\/td>\n      <\/tr>\n      <tr>\n        <td id=\"file-gistfile1-txt-L4\" class=\"blob-num js-line-number\" data-line-number=\"4\"><\/td>\n        <td id=\"file-gistfile1-txt-LC4\" class=\"blob-code blob-code-inner js-file-line\">                add_header &#39;Access-Control-Allow-Methods&#39; &#39;GET, POST, OPTIONS&#39;;<\/td>\n      <\/tr>\n      <tr>\n        <td id=\"file-gistfile1-txt-L5\" class=\"blob-num js-line-number\" data-line-number=\"5\"><\/td>\n        <td id=\"file-gistfile1-txt-LC5\" class=\"blob-code blob-code-inner js-file-line\">                add_header &#39;Access-Control-Allow-Origin&#39; &#39;*&#39;;<\/td>\n      <\/tr>\n      <tr>\n        <td id=\"file-gistfile1-txt-L6\" class=\"blob-num js-line-number\" data-line-number=\"6\"><\/td>\n        <td id=\"file-gistfile1-txt-LC6\" class=\"blob-code blob-code-inner js-file-line\">        }<\/td>\n      <\/tr>\n      <tr>\n        <td id=\"file-gistfile1-txt-L7\" class=\"blob-num js-line-number\" data-line-number=\"7\"><\/td>\n        <td id=\"file-gistfile1-txt-LC7\" class=\"blob-code blob-code-inner js-file-line\">\n<\/td>\n      <\/tr>\n      <tr>\n        <td id=\"file-gistfile1-txt-L8\" class=\"blob-num js-line-number\" data-line-number=\"8\"><\/td>\n        <td id=\"file-gistfile1-txt-LC8\" class=\"blob-code blob-code-inner js-file-line\">}<\/td>\n      <\/tr>\n<\/table>\n\n  <\/div>\n\n  <\/div>\n  \n<\/div>\n\n      <\/div>\n      <div class=\"gist-meta\">\n        <a href=\"https://gist.github.com/jaxgeller/4a0a66919b14f3a8f2ec/raw/316cbaef65b428ea1742037a4d823a2a5dfbf533/gistfile1.txt\" style=\"float:right\">view raw<\/a>\n        <a href=\"https://gist.github.com/jaxgeller/4a0a66919b14f3a8f2ec#file-gistfile1-txt\">gistfile1.txt<\/a>\n        hosted with &#10084; by <a href=\"https://github.com\">GitHub<\/a>\n      <\/div>\n    <\/div>\n<\/div>\n')
{% endhighlight %}

This is bad because the script will try to overwrite the entire document.

So instead of this method, I had to chose another. I decided on JSONP, which would allow me to get a JSON response from github with details of the gist.

{% highlight javascript %}
/**/
jQuery21408490260478574783_1442096787578({
    "description": "Run blank ubuntu instance via docker",
    "public": true,
    "created_at": "2015-07-10T20:42:34-04:00",
    "files": ["blank_instance"],
    "owner": "jaxgeller",
    "div": "<div id=\"gist24175800\" class=\"gist\">...content omitted...</div>",
    "stylesheet": "https://assets-cdn.github.com/assets/gist/embed-3313cb70789df61d3bb75cd878325a1266ccab50b6aac9b80f1691ae0694a216.css"
});
{% endhighlight %}

The code for that looks like this. (note I used jquery for their jsonp implementation)

{% highlight javascript%}
var URL = "https://api.github.com/users/jaxgeller/gists";
var container = document.querySelector('.post-content');

$.get(URL, function(r) {
  r.forEach(function(gist){
    $.getJSON("https://gist.github.com/jaxgeller/"+gist.id+".json?callback=?", function(data) {
      var snippet = document.createElement('div');
        snippet.classList.add('snippet');

      var title = document.createElement('h3');
        title.textContent = gist.description;

      var content = document.createElement('div');
        content.innerHTML = data.div

      var sheet = document.createElement('link');
        sheet.href= data.stylesheet;
        sheet.rel = "stylesheet";

      snippet.appendChild(title);
      snippet.appendChild(sheet);
      snippet.appendChild(content);
      container.appendChild(snippet)
    });
  });
});
{% endhighlight %}

That's all for this post, once again if you'd like to see an example of it working checkout [this page.](http://jaxgeller.com/snips/)
