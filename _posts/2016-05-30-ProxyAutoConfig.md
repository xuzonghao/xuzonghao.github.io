---
layout: post
title:  "PAC(Proxy Auto Config) Script"
date:   2016-05-30
categories: script
---


<?php
function FindProxyForURL(url, host)
{

if (isPlainHostName(host))
    return "DIRECT";
else if (shExpMatch(host, "twitter.com"))
    return "PROXY 10.81.11.72:3128";
else if (shExpMatch(host, "twimg.com"))
    return "PROXY 10.81.11.72:3128";
else if (shExpMatch(host, "t.co"))
    return "PROXY 10.81.11.72:3128";
else if (shExpMatch(host, "*.sina.com"))
    return "PROXY 10.210.97.118:3128";
else if (shExpMatch(host, "*.sina.cn"))
    return "PROXY 10.210.97.118:3128";


url = url.toLowerCase();
if (shExpMatch(url, "*.google*.com*")||
    shExpMatch(url, "*.buzzfeed.com*")||
    shExpMatch(url, "*.wordpress.com*")||
    shExpMatch(url, "*.softether-download.com*")||
    shExpMatch(url, "*.softether.org*")||
    shExpMatch(url, "*.maven.org*")||
    shExpMatch(url, "*.bintray.com*")||
    shExpMatch(url, "*.openvpn.net*")||
    shExpMatch(url, "*.openx.net*")||
    shExpMatch(url, "*.arcpublishing.com*")||
    shExpMatch(url, "*.washingtonpost.com*")||
    shExpMatch(url, "*.scorecardresearch.com*")||
    shExpMatch(url, "*.dowjoneson.com*")||
    shExpMatch(url, "*.typekit.net*")||
    shExpMatch(url, "*.tipcdn.com*")||
    shExpMatch(url, "*.youtu.be*")||
    shExpMatch(url, "*.dropbox.com*")||
    shExpMatch(url, "*.dropboxstatic.com*")||
    shExpMatch(url, "*.ap.org*")||
    shExpMatch(url, "*.ggpht.com*")||
    shExpMatch(url, "*.t.co*")||
    shExpMatch(url, "*.akamaihd.net*")||
    shExpMatch(url, "*.instagram.com*")||
    shExpMatch(url, "*.tiqcdn.net*")||
    shExpMatch(url, "*.outbrain.com*")||
    shExpMatch(url, "*.bbc.co.uk*")||
    shExpMatch(url, "*.apne.ws*")||
    shExpMatch(url, "*.bbc.com*")||
    shExpMatch(url, "*.static-economist.com*")||
    shExpMatch(url, "*.nytimes.com*")||
    shExpMatch(url, "*.truste.com*")||
    shExpMatch(url, "*.wikipedia.org*")||
    shExpMatch(url, "*.wikimedia.org*")||
    shExpMatch(url, "*.igodigital.com*")||
    shExpMatch(url, "*.vidora.com*")||
    shExpMatch(url, "*.chartbeat.com*")||
    shExpMatch(url, "*.reutersmedia.net*")||
    shExpMatch(url, "*.zqtk.net*")||
    shExpMatch(url, "*.cnn.com*")||
    shExpMatch(url, "*.revsci.net*")||
    shExpMatch(url, "*.google.co.jp*")||
    shExpMatch(url, "*.google.co*")||
    shExpMatch(url, "*.bloomberg.com*")||
    shExpMatch(url, "*.fbcdn.net*")||
    shExpMatch(url, "*.reuters.com*")||
    shExpMatch(url, "*.wsj.com*")||
    shExpMatch(url, "*.economist.com*")||
    shExpMatch(url, "*.time.com*")||
    shExpMatch(url, "*.timeinc.net*")||
    shExpMatch(url, "*.googlevideo.com*")||
    shExpMatch(url, "*.ytimg.com*")||
    shExpMatch(url, "*.googleapis.com*")||
    shExpMatch(url, "*.wp.com*")||
    shExpMatch(url, "*.gmail.com*")||
    shExpMatch(url, "*.youtube.com*")||
    shExpMatch(url, "*.facebook.com*")||
    shExpMatch(url, "*.googlecode.com*")||
    shExpMatch(url, "*.sourceforge.net*")||
    shExpMatch(url, "*.uberconference.com*")||
    shExpMatch(url, "*.yahoo.com*")||
    shExpMatch(url, "*.neofonie.de*")||
    shExpMatch(url, "*.yahoo.co*")||
    shExpMatch(url, "*.doubleclick.net*")||
    shExpMatch(url, "*.yimg.com*")||
    shExpMatch(url, "*.hk.yahoo.com*")||
    shExpMatch(url, "*.cloudfront.net*")||
    shExpMatch(url, "*.gstatic.com*")||
    shExpMatch(url, "*.syndication.com*")||
    shExpMatch(url, "*.spotxchange.com*")||
    shExpMatch(url, "*.mathtag.com*")||
    shExpMatch(url, "*.cloudflare.com*")||
    shExpMatch(url, "*.android.com*"))
    return "PROXY 10.81.11.72:3128";
else  
    return "DIRECT";
}

