![image](https://s.perimeterx.net/logo.png)

# [PerimeterX](http://www.perimeterx.com) Advanced Blocking Response Samples

Advanced Blocking Response Example
----------------------------------

> This folder contains an example of the Advanced Blocking Response implementation.

In special cases, (such as XHR post requests) a full Captcha page render might not be an option. In such cases, using the Advanced Blocking Response returns a JSON object continaing all the information needed to render your own Captcha challenge implementation, be it a popup modal, a section on the page, etc. The Advanced Blocking Response occurs when a request contains the Accept header with the value of application/json. 

In order to use this example:

1. Create a block.html file in your application (or copy the one in this folder).   
 The `<body>` section **must** include (replacing <APP_ID> with your  PX App ID):

```html
   <script>
       window._pxAppId = '<APP_ID>';
       window._pxJsClientSrc = 'https://client.perimeterx.net/<APP_ID>/main.min.js';
       window._pxHostUrl = 'https://collector-<APP_ID>.perimeterx.net';
   </script>
   <script src="https://captcha.px-cdn.net/<APP_ID>/captcha.js?a=c&m=0"></script>
```
* In the HTML structure, the `<body>` section must include the following line where the Captcha element is to be located:

```
   <div id="px-captcha"></div>
```

2. Set the `_M.custom_block_url` to the location you have just defined (e.g. /block.html).
4. Set the `_M.redirect_on_custom_url` flag to **false**.
5. Change the `<APP_ID>` placeholder on the block.html page to the Application ID provided on the PerimeterX Portal.


A sample JSON response appears as follows:

```
   {
       "appId": String,
       "jsClientSrc": String,
       "firstPartyEnabled": Boolean,
       "vid": String,
       "uuid": String,
       "hostUrl": String,
       "blockScript": String
   }
````

Once you have the JSON response object, you can pass it to your implementation (with query strings or any other solution) and render the Captcha challenge.

In addition, you can add the `_pxOnCaptchaSuccess` callback function on the window object of your Captcha page to react according to the Captcha status. For example when using a modal, you can use this callback to close the modal once the Captcha is successfullt solved. 
An example of using the `_pxOnCaptchaSuccess` callback is as follows:

```
   window._pxOnCaptchaSuccess = function(isValid) {
       if(isValid) {
           alert("yay");
       } else {
           alert("nay");
       }
   }
```   
