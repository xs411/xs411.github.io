---
layout: post
title: "Using Cross Origin Resource Sharing (CORS) Securely"
date: 2013-09-14 17:54
comments: true
categories: 
---
Recently, I've run across some applications using [Cross Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-Origin_Resource_Sharing). I needed to study up on the topic and realized this is a feature that needs to be implemented with care. Below I've document what I learned in hopes that it will help others.

##What is Cross Origin Resource Sharing?##
Cross Origin Resource Sharing is a method of sharing data between two domains using JavaScript with HTTP headers under the hood. This normally would not be allowed due to the [Same Origin Policy](http://www.w3.org/Security/wiki/Same_Origin_Policy). However, developers often need the ability to share data between two domains(websites) and thus Cross Origin Resource Sharing was added with the release of HTML 5.

##How Do I use Cross Origin Resource Sharing?##
A very good explanation of the details of using CORS is explained here: [http://www.html5rocks.com/en/tutorials/cors/](http://www.html5rocks.com/en/tutorials/cors/). At a high level, you use JavaScript to make a request to another domain. The second domain responds with special headers that specify the authorization policy. If the request is authorized, the resource requested is returned to the page with the headers. Then JavaScript event handlers are triggered to handle the processing of the data.

##Security Concerns##
When setting your CORS policy, you must configure a policy that makes sense for the data and functionality that is specific to your website and the needs of the business. The key concerns are:

1. Don't enable CORS on resources you don't want to share across domains.
2. Only grant access to domains that need access.
3. If the data is sensitive, use appropriate supporting controls:
	A. Use authentication and the "withCredentials" property.
	B. Use a secure (HTTPS) connection.

By following the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) and using the correct level of authorization for the data that is being returned these risks can be avoided.

##Security Solutions##
Use the following features of CORS to avoid security concerns (2) and (3A).

###Access-Control-Allow-Origin###

><code>
>Access-Control-Allow-Origin: *
></code>

This policy states that any other website can pull information from the responding website. Instead you should specify a specific domain with which you explicitly want to share data. Example:

><code>
>Access-Control-Allow-Origin: http://securio.us
></code>

Additionally, the server should be configured to only respond to CORS requests for specific resources that you want to share. So, rather than setting up CORS requests for an entire domain (ex: http://securio.us), only setup Cross Origin Resource Sharing on a specific page (ex: http://securio.us/public). There isn't really a "one size fits all" approach because different sites will need to share their data differently. If the site has no private data/functionality an open policy might actually make sense, though this is generally a bad idea as it is not future proof. 

For example, today your site might consist of only public resources. However, if you decide to add a part of the site that . Its a best practice to explicitly state what data is allowed to have access and prevent some data that is public from getting out, than saying everything is open and allowing a lot of private data to leak out. Just keep in mind the security principle of least privilege and only allow access where it is needed.

###withCredentials###

What about when you have data or functionality that is sensitive, but you still want to make it accessible across domains? The "withCredentials" property can be used for just such a scenario. 

><code>
>xhr.withCredentials = true;
></code>

This will instruct the browser to send cookies with the request. Then, the server can verify that the request is authorized. 

##Summary##

CORS can be used to solve a common problem in security. However, keep in mind that the Same Origin Policy was put in place to protect users. Cross Origin Resource Sharring should be used as a seasoning for your websites not the main course. Try not to get carried away in using it everywhere.

##References##
* [Cross-Domain AJAX with Cross-Origin Resource Sharing](http://www.nczonline.net/blog/2010/05/25/cross-domain-ajax-with-cross-origin-resource-sharing/)
* [Using CORS Tutorial](http://www.html5rocks.com/en/tutorials/cors/)

