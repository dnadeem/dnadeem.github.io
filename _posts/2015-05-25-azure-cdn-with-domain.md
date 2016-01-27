---
layout: post
title: "How to setup Azure CDN and attach it to a custom domain."
tags:
- Azure
- CDN
thumbnail_path: blog/azure-cdn/azure.png
---
How to setup Azure CDN and attach it to a custom Domain

{% include figure.html path="blog/azure-cdn/azure.png" link_to_full_size_image=false %}

**Step 1:** Create a new account or login to [Azure](https://azure.com) and determine what you wish to accelerate. This can either be storage, a website, or a cloud service.

**Step 2:** Navigate to the CDN icon and click New
{% include figure.html path="blog/azure-cdn/1.png" caption="Click on CDN" link_to_full_size_image=true %}

**Step 3:** Click Quick Create, then choose the origin domain (what you wish to speed up). For this tutorial I chose my Blob storage account because it contains all the assets such as images, site files, and other static content.

{% include figure.html path="blog/azure-cdn/origin.png" caption="Select the origin domain" link_to_full_size_image=true %}


**Step 4:** Upon successful creation you are met with the settings page. Here you can enable HTTPS and mask the *.vo.msecnd.net domain

{% include figure.html path="blog/azure-cdn/3.png" caption="Click enable HTTPS if you wish" link_to_full_size_image=true %}


**Step 5 (optional):** Click &#8220;Manage Domains,&#8221; and add in your domain name. For example: cdn.mydomain.com, then go into your domain/dns provider and create a CNAME record that points from cdn.mydomain.com to the CDN endpoint given in Azure.

I use Cloudflare as my DNS host. Your settings should match below. Allow at least 1 hour then click the check box in Azure.

{% include figure.html path="blog/azure-cdn/cloudflare.png" caption="DNS settings" link_to_full_size_image=true %}

**NOTE:** Cloudflare acceleration must be disabled. i.e. the cloud should be gray. 
When using storage as the origin account it takes 1-2 hours for files to appear on the CDN because they must be cached on servers worldwide.

You should now replace static file URLs on your websites to the CDN to allow for almost instantaneous loading.
[Current Azure CDN pricing](http://azure.microsoft.com/en-us/pricing/details/cdn/)
       
	
