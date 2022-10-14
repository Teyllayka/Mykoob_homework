```js
// ==UserScript==
// @name         wix_none_Premium_FIX
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://agitac.wixsite.com/*
// @icon         https://www.google.com/s2/favicons?sz=64&domain=wixsite.com
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    document.getElementById("WIX_ADS").style.display = "none";
    document.querySelector("#site-root").style.top = "unset"
})();
```
