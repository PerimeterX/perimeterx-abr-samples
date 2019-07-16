![image](https://s.perimeterx.net/logo.png)

# [PerimeterX](http://www.perimeterx.com) Advanced Blocking Response Examples

> This folder contains examples of how to implement Advanced Blocking Response.

## What Is Advanced Blocking Response?

In special cases, (such as XHR post requests) a full Captcha page render might not be an option. In such cases, using the Advanced Blocking Response returns a JSON object continaing all the information needed to render your own Captcha challenge implementation, be it a popup modal, a section on the page, etc.

Advanced Blocking Response occurs when a request that is marked for blocking contains an `Accept` header with the value of `application/json`. It returns a JSON with the following structure:

```javascript
{
    "appId": String,
    "jsClientSrc": String,
    "firstPartyEnabled": Boolean,
    "vid": String,
    "uuid": String,
    "hostUrl": String,
    "blockScript": String
}
```

## Minimum Requirements

To render the challenege element (be it Human Challenge or reCaptcha) you must have the following elements/snippets in your implementation:

### The Window Object Properties

The element that renders the challenge must contain the following `window` object properties:

```javascript
<script>
    window._pxAppId = '<appId>'; // PerimeterX's application id
    window._pxJsClientSrc = '<jsClientSrc>'; // PerimeterX's JavaScript sensor url
    window._pxFirstPartyEnabled = <firstPartyEnabled>; // A boolean flag indicating wether first party is enabled or not
    window._pxVid = '<vid>'; // PerimeterX's visitor id
    window._pxUuid = '<uuid>'; // PerimeterX's unique user id
    window._pxHostUrl = '<hostUrl>'; // PerimeterX's cloud component URL
</script>
```

All of these properties should be taken from the Advanced Blocking Response JSON response.

### The Challenge Element

The challenge element will render to a `div` with an id of `px-captcha`:

```html
<div id="px-captcha"></div>
```

### Loading the Block Script

Add the block script to rendering element either by a `script` tag or by dynamically loading it to the Head element:

```javascript
let script = document.createElement('script');
script.src = response.blockScript; // use the blockScript property from the Advanced Blocking Response result.
document.getElementsByTagName('head')[0].appendChild(script);
```

## Challenge Result Callback

You can add the `_pxOnCaptchaSuccess` callback function on the `window` object of your challenge page to react according to the challenge status. For example when using a modal, you can use this callback to close the modal once the challenge is successfullt solved.

The `_pxOnCaptchaSuccess` callback can be used as follows:

```javascript
   window._pxOnCaptchaSuccess = function(isValid) {
       if (isValid) {
           alert("Challenge Successful");
       } else {
           alert("Challenge Failed");
       }
   }
```
