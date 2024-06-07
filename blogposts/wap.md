![](https://cdn.hashnode.com/res/hashnode/image/upload/v1712345353782/bebaa827-df90-4d49-a68e-198c89f7027a.png?w=1600&h=840&fit=crop&crop=entropy&auto=compress,format&format=webp)
# 🕷️ Web Pen-testing strategies
> The ultimate primer to web application penetration techniques

In this blog, we will discuss most concepts concerning and related to web app security. Things like Auth-bypass, SSRF,etc. Massive thanks to portswigger for the inspiration.

## WAPT in a nutshell

WAPT is nothing but looking for faults in the backend system of the application and then exploiting it. We use several tools like fuzzers, proxies and vulnerability scanners for this (I'll get into that later).

## Tools for WAPT

### Fuzzers

Fuzzers are applications that can send the same request repetitively with minor edits. The edits are taken from an existing wordlist. Let's say we have a URL:

```plaintext
https://website.com/
```

Now we need to find the various API endpoints for this. So we will use a fuzzer to send requests with various edits to the website URL and hope for a success return code. (Here are the HTTP return codes for reference)

*HTTP Return codes (Signals of API responses)*

* 2xx - Success
    
* 3xx - Redirection
    
* 4xx - Client side error
    
* 5xx - Server side error
    

the fuzzer will use a wordlist that may be something like this:

```plaintext
v2
swagger
control-panel
...
```

Now the requests being sent will look like this

```plaintext
GET https://website.com/api/v2 [RETURN CODE 403]
GET https://website.com/api/swagger [RETURN CODE 403]
#this one exists!
GET https://website.com/api/control-panel [RETURN CODE 200]

...
```

Common examples of Fuzzers include Ffuf, Burp intruder and GoBuster (which is specialised for directory enumeration)

### Proxies

Proxies are tools that sit in the middle of the website and the user's browser. They capture the HTTP history of communication between the user and the website.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1711706001557/33b1b202-5f83-4045-91bc-0a1170333cce.png)

A common example of the proxy tool is Burp-Suite, the gold standard for WAPT.

### Vulnerability scanners

These tools assess the website for possible vulnerabilities and can generate things like phishing websites or SQL syntax to exploit it. Common tools are Metasploit (gold standard), XSRFProbe (For CSRF-based exploits), SQLMap (For SQLi based exploits)

## Dealing with internal APIs

Internal APIs deal with requests and responses sent between the website and the web-server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1711708656523/5a9194a2-13d0-4dcc-82c9-bb0e099048d0.png)

## API Recon

The aim is to find as many endpoints as possible. This is an intermediate stage before exploitation. There various types of HTTP requests, here's a list of common ones:

* GET -&gt; Ask the webserver for a resource
    
* POST -&gt; Add new data
    
* PATCH/PUT -&gt; Change existing data
    
* OPTIONS -&gt; Options for types of requests allowed
    

Through sending requests, we can explore these from the responses:

* Parameters (Hidden ones too)
    
* Type of content accepted
    
* Type of request accepted
    

## Types of vulnerabilities

### File upload and webshell attacks

Sometimes the file upload part of the website can be used to upload malicious scripts onto the web server and executed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1711706542616/a8b8bd53-73dd-4c9a-9b3e-45b475ab20b0.png)

This can be exploited if there is no `Content-Type` header restriction on the API request, meaning the website does not check if the uploaded file is an image (as intended) or a PHP script.

When we are dealing with things like this, the most common types of attacks are webshell attacks. In which shell commands can be executed in the web server using languages like PHP.

Here's an example command:

```php
<?php echo passthru($_GET['cmd']); ?>
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1711706601232/d48a6591-4461-4680-9440-d37919e3f7c1.png)

### Continued: Flawed file type validation

To counter this, website creators do add the content-type header. But what the request can be edited before it is send? We can capture the outgoing request through burp, and change the content-type restriction from the outgoing script to setting it as an "image"

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1711706826048/4e494cd1-28ac-406e-9e8b-65fe141d365b.png)

## Server Side Request Forgery

Server Side Request Forgery (SSRF) is a technique where requests sent by the website to the web-server are edited to gain access to sensitive info (usually).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1711707235111/2405042f-dc8e-4b95-b6dd-16efec24754c.png)

## 2FA bypass

This is usually a rare case but sometimes the user is already logged in during login before they enter their MFA code.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1711707440601/61a2bb15-3778-4d79-9d5d-0d8d0dcffe06.png)

## Privilege escalation

In this technique we check if we can access other user's profiles through ours. We make our way into the admin dashboard through this technique. We can achieve this by editing the search query in the URL or editing the parameters in the requests.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1711708032214/a9d6e23a-34ba-4223-a5a8-7e542975cd2f.png)

We can also explore through the source code (usually JS) too look for clues for api endpoints to the admin panel

like this:

```plaintext
https://website.com/login/home?admin=false
#now set this to true
https://website.com/login/home?admin=true
```

Sometimes we may also find these parameters (like `admin`) in the HTTP history when exploring through the responses.

## SQL injection

Structured Query Language is a language used to query and manipulate databases with ease. However when integrated into web-servers, we can forge SQL commands that can be put into search boxes to send an SQL command through the internal API to the web-server and return the query result instead of a normal one.

Here's an example, if we have a login page requiring username and password and the user account existence is checked by this command in the web-server:

```plaintext
SELECT * FROM USERS WHERE username="superman" AND password="m4sterh4xx0r";
```

Then if it exists, the website logs the user in. We can exploit it.

We can input an SQL statement in the strings and edit the query sent through the internal API

```plaintext
SELECT * FROM USERS WHERE username="superman"--" AND password="m4sterh4xx0r"; 
```

here, we have input "-- into the username string where " closes the string and -- comments out the rest of the query, so the password check is simply ignored and lets us in.

However, for an SQLi to be successful, we need to be vary of the technology of SQL the web-server is using, its version and the columns which accept string input (also the table name sometimes). Usually websites tend to use MySQL, MariaDB, PostgreSQL or Oracle Database

## Cross-site request forgery

This technique is usually related to using the user's existing session to change their credentials and login.

The most common application of CSRF is related (but not entirely encompassing) to phishing.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1711709575953/4a9ffdfb-c6c5-4bea-a600-5d0bd96cab26.png)

A user's login session (also the time span the website thinks that the user is logged in) is determined by a session cookie. It is a token. However if a hacker manages to get hold of it, they can send requests with the user's session cookie to change their credentials and login as the user (because the website thinks the hacker is the user, by looking at the session cookie!).

There are thankfully, safeguards to this:

* Additional `csrf token` parameter whose value is randomly generated every session
    
* Measures to make sure that the generated CSRF token is tied to only that particular user's session
    

If either of these is absent, it becomes easy for the hacker to attempt a CSRF attack.

# References:

* [https://github.com/JohnTroony/php-webshells](https://github.com/JohnTroony/php-webshells)
    
* [https://portswigger.net/web-security](https://portswigger.net/web-security)
    
* [https://portswigger.net/burp](https://portswigger.net/burp)
    
* [https://github.com/ffuf/ffuf](https://github.com/ffuf/ffuf)
    
* [https://www.metasploit.com/](https://www.metasploit.com/)
    
* [https://github.com/0xInfection/XSRFProbe](https://github.com/0xInfection/XSRFProbe)
    
* [https://excalidraw.com/](https://excalidraw.com/)