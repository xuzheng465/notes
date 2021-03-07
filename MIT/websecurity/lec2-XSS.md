代理有能力阻止请求发出，等待用户转发请求



### How Do Proxies Deal With Encrypted Protocols?

Proxies require a root certificate to be installed within the client (your browser in this case)

代理需要在客户端安装一个根证书（这里是指浏览器）。



### LocalStorage 本地存储

Websites can store information within your browser for later

Can be used for managing state

No expiration

Accessible via JS

Persists after browser closes

Was an old method for enforcing bans on websites

Maximum storage space is 5MB



### DOM

* Document Object Model
* Parsing of HTML and XML
* Object-oriented representation of web pages
* Can be controlled by scripting languages like JS
  * Document object in JS



### What is user input? 

Literally anything the user can control

• Examples:

• IP addresses

• Hosts

• Cookies

• Local storage

• GET parameters

• POST parameters

• And many, many other things.



### Intro Cross-Site Scripting (XSS)



* Execution of JavaScript within the client’s browser

* Attacker’s aim is to:

  • Steal cookies 窃取cookies

  • Steal DOM 

  • Steal localStorage 窃取本地存储

  • Manipulate AJAX calls to perform unauthorised operations

* It uses the victim’s **session tokens**



### Reflected XSS

1. Attacker sends malicious link 

   攻击者发送“有毒”链接

2. users clicks the link and it is executed in the browser

   用户点击链接，链接在浏览器中运行。

3. Browser sends the private data to the attacker

   浏览器发送私密数据给攻击者



### Stored XSS

1. Perpetrator discovers a website having a vulnerability









































