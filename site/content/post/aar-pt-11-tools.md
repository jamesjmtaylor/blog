---
title: AAR pt 11 (Tools)
date: '2019-02-01T11:15:55-08:00'
---
![Tools](/img/blog/tools.jpg)

If you have not had a chance to read the first entry in the series for context, <a href="/post/after-action-review-aar/">you can do so here</a> 

It's been a almost exactly a year since I did <a href="/post/aar-pt-6-tools/">my last Tools AAR entry</a>, so I figured it was about time that I had a new entry on my lessons learned.  This post focuses almost exclusively on security tools.

* You can sunset orphan apps with token checks and token formatting tied to versions as long as you implement that logic from the first launch of the app.
* Swagger is a auto-documenting API tool implemented through a number of API language libraries (i.e. Swashbuckle for C# MVC).  If reading through a Swagger document always look at the "Try it out!" to see the curl request format.
* Kibana is a front-end display for elastic search, which is used to index your database for queries.  Elastic search can also be used as a database itself.
* Checkmarx integration scans code-base repositories for known security vulnerabilities. I've personally seen it catch security vulnerabilities in iOS, Android, and C# MVC applications.
* Sonatype integration allows you to scan open source dependencies for licensing and security issues, as well as their nested sub-dependencies.  For example, it will alert you if one of your external libraries is licensed under the GPL. GPL licensing is known as "copy-left", which requires code that uses it to be open-source.  This is generally not feasible for enterprise software solutions.
* TwistLock scans Docker containers for common security vulnerabilities.  As an example it might tell you that your container defaults users to root privileges (which is actually fairly standard for docker containers).
* SSH and HTTPs both use TLS for encryption and decryption of traffic.  To break 2048-bit TLS encryption it would take a 2.3 Ghz CPU using NFS (Number Filter Seive) attack 6.8 quadrillion years to crack.
* Github allows you to authenticate either with username and password over HTTPS or by SSH key. By using SSH keys you can limit which keys can push to which repositories.
* AES stands for Advanced Encryption Standard.  It's a 256-bit symmetric key.
* HMAC stands for Hash-Based Message Authentication Code.  It's an encryption code generated by hashing a message with a devices AES/Network Key.
* Socket authentication is done either through the url or as the 1st payload sent over the socket.  Normal sockets do NOT use TLS, just TCP, and special libraries must be used to gain the benefits of TLS for sockets.

<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px" href="https://unsplash.com/@randyfath?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="_blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from Randy Fath"><span style="display:inline-block;padding:2px 3px"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-2px;fill:white" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M10 9V0h12v9H10zm12 5h10v18H0V14h10v9h12v-9z"></path></svg></span><span style="display:inline-block;padding:2px 3px">Photo by Randy Fath</span></a>
