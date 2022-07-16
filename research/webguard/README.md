# WebGuard Research Material

[https://link.springer.com/chapter/10.1007/978-3-030-35653-8_33](Original paper)
```
MyWebGuard: A User‑Oriented Approach and Tool for Security and Privacy
Protection on the Web

Phu H. Phung · Huu‑Danh Pham · Jack Armentrout 
Panchakshari N. Hiremath · Quang Tran‑Minh
```

## Javascript Security Examples

* BrowserShield

Rewrites web pages to secure vulnerabilities relied on embedded scripts.

* Google Caja

Rerwite scripts into object-capability language; passive data becomes active
content this way.

* ConScript

Host webpages by fine-grained security policies.

* FlashJaX

Javascript + Adobe ActionScript content security.

* Advertisement Networks:
1. ADSafe: Javascript for ads.
1. Politz: Verify sandbox sources (fixes vulners in ADSafe).
1. Finifter: Third party exploiting prototype objects of hosting page.
1. Privad: Targeted ads, more secure browsing experience
1. RePriv: Tracks user data and interest, with users permission.

* Chudnov, Naumann DA

Inlined information flow monitoring for Js; for sure having a look at this
first.

* JSFlow

Tracking information flow of Js on sites that utilize third-party code, deploys
as a browser extension. 

* GUARDIA

Develop framework for enforcing secuirty policies for Javascript web
applications.

* ScriptProtect

Prevent XSS by using Js to prevent string-to-code conversions; preventing first-
party origin from XSS.

* WebGuard

What I'm looking to do :D ^ All the above will be used for research purposes to
understand the local environment and provide necessary foundational information
as to what will work, why certain things should not be looked into further due
to not providing any use for 'x' reasons. WebGuard will essentially plant
security clearances at gateways of *sinks and sources.* This will be the main
idea, everything coming in and passing out will be monitored one way or another.
First we will look into logging and monitoring all this information, hopefully
we have some time to implement some policies for testing. If that does not come
to be because of timing reasons within project deadline[??] we will continue to
work on related things, and continuing to learn :)

## Overview

The entire idea here is to monitor Js execution client-side. We want to track
method calls and property access. Idea of sinks and sources here applies heavily
when we call a function what happens in terms of data transfer from method call
to method execution, and finally where the computation outcome is used. We will
essentially hire a bunch of robots and place them in key specific key access
points to make sure they abide by *our policies*.

Js intercept operations:
1. Method calls			==> functions of global objects (getElementById)
1. Object creation and access	==> 
1. Property access		==> field of objects (cookie)

```Javascript
(function() {
	let reference = original;
	let code_origin = getCodeOrigin(..);
	original = wrapper() {
		if (PolicyCheck(code_origin, reference_name, arguments))
			execute(reference);
		}
	}) ();
```

**What happens above?** We define an anonymous function that gets & sets the js
executing code to the variable 'code\_origin'. We will track this code\_origin
as it tries to do the operations mentioned above.

```Javascript
var desc = Object.getOwnPropertyDescriptor(Document.prototype, "cookie");

Object.defineProperty(document, "cookie", {get: function(){
    var code_origin = getCodeOrigin(new Error().stack);
    if (originAllowed(code_origin, "cookie")) {
        setOriginSourceRead(code_origin, "cookie");
	return desc.get.call(document);
    }
    return;
  },
  set: function(val){ desc.set.call(document,val); },
  enumerable : false,
  configurable : false
});
```

**What happens above?** We use the Object.defineProperty API to set policies for
get and set operations to the specified field. The field in this case is the
'cookie' field. 

```Javascript
var imgPolicy = {
    get: function(obj, prop) { /* set policies for get operation */ },
    set: function(obj, prop, value) { /* set policies for get operation */ }
};
var OriginalImage = Image;
class ImageWrapper {
    construction(height, width) {
        var imgObject = new OriginalImage(height, width);
	imgObject = new Proxy(imgObject, imgPolicy);
	return imgObject;
    }
}
Image = ImageWrapper;
```

**What happens above?** Creating new wrapper that gets mediated by proxy object.
This is due to equivelant function calls not being captured, so we define it 
ourselves.

## Context Aware Policies

Ideal security mechanism is to control the entire information flow using 
policies, but we cant do that. So we use endpoint security instead. Anything
that tries to read from sensitive data sources or tries to send data outside of
a browser we will monitor. Because we keep track of the code origin we can
enforce policies that are context aware.

## User-centric policies

We have different strictness levels that users can opt-in for using the popup
ui. Some function calls made available to all code origins is a security risk 
so depending on the strictness level, some may be allowed to call and others not.
Users can also blacklist origins they dont trust.

## Js Monitoring Library

We develop a library that will intercept javascript operations; split these
types of operations into 2:

1. Data Source Access: get data
1. Data sink channels: send data

### Data Source Access

This is an umbrella for all access to: cookie, html local storage, browsing
history, location, value of form elements and web page contents. Make sure
whether code origin is allowed to read, if so mark as read by code origin

```Javascript
function localStorage_getItem_policy(args, proceed, obj) {
    var itemID = args[0];
    var code_origin = getCodeOrigin(new Error().stack);
    if (originAllowed(code_origin, "localStorage", "getItem", itemID)) {
        setOriginSourceRead(code_origin, "localStorage");
        return proceed();
    }
    return;
}
monitorMethod(localStorage, "getItem", localStorage_getItem_policy)
```

**What happens above?** we will get the policy regarding localstorage access,
then we will check whether the code origin is allowed to access this item
by passing code origin, "localstorage", "getitem", and item id to the function
`originAllowed`. If it is, then set `setOriginSourceRead` and return.

What calls can access user data:

1. Document.getElement
1. localStorage.getItem
1. document.cookie
1. window.history
1. navigator.geolocation.getCurrentPosition

All the above are possible access locations we will/need to monitor. Most are
method calls, some access cookie and local, the obvious one access geolocation.

### Data Sink Channels

This research looks particular only at data sent outside from the browser and
not redirection attacks and similar things. Maybe for our work we may implement
something here?? Seems like an interesting gap we can fill between network
based and javascript based runtime checkers.

## MyWebGuard - Privacy Protection Tool in Browsers

Library is implemented server and client side - we want to further look at
developing a browser extension that will take care of all of this automatically.
We do this by insuring our code appended to innerHTML runs before the webpage
does. We will perform experiments that our code is executed before any other
code in web page.

```Javascript
var mywebguard = document.createElement("script");
mywebguard.innerHTML = '
(function() {
    // java interception here
})();';
document.documentElement.appendChild(mywebguard);
```

This extension will be developed first for chromium based browsers, so chrome
and brave as well as other derivatives of chrome will work.

## End Remarks

Continuing on from here is the conclusion of the original creators work. We will
not summarize and write any of it as to have the user look at it for themselves.
Regardless we are not here to summarize and share the outcomes of other peoples
work but to try understand and potentially improve, or at the very least modify.
Original creators of this paper and extension are named at the top of this
markdown file, have a look.
