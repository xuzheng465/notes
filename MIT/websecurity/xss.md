# Cross-site scripting

在本节中，我们将解释什么是跨站脚本(cross-site scripting)， 描述不同种类的跨站脚本漏洞，并阐明如何发现和预防跨站脚本。

## 什么是跨站脚本(XSS)

跨站脚本（也称为XSS）是一种网络安全漏洞，允许攻击者破坏用户与易受攻击的应用程序的交互。它允许攻击者规避**同一来源策略**，<u>该策略旨在将不同的网站相互隔离</u>。跨站脚本漏洞通常允许攻击者伪装成受害者用户，执行用户能够执行的任何操作，并访问用户的任何数据。如果受害者用户在应用程序中拥有特权访问权限，那么攻击者可能会完全控制应用程序的所有功能和数据。

## XSS如何工作？

跨站脚本的工作原理是操纵一个脆弱的网站，使其向用户返回恶意的JavaScript。当恶意代码在受害者的浏览器内执行时，攻击者可以完全破坏他们与应用程序的互动。



## What are the types of XSS attacks?

- [Reflected XSS](https://portswigger.net/web-security/cross-site-scripting#reflected-cross-site-scripting), where the malicious script comes from the current HTTP request.
- [Stored XSS](https://portswigger.net/web-security/cross-site-scripting#stored-cross-site-scripting), where the malicious script comes from the website's database.
- [DOM-based XSS](https://portswigger.net/web-security/cross-site-scripting#dom-based-cross-site-scripting), where the vulnerability exists in client-side code rather than server-side code.

## What can XSS be used for?

* 冒充或伪装成受害者用户。
* 执行用户能够执行的任何行动。
* 读取用户能够访问的任何数据。
* 捕获用户的登录凭证。
* 对网站进行虚拟污损。
* 在网站中注入木马功能。



## Impact of XSS vulnerabilites

