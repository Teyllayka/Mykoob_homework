```js
// ==UserScript==
// @name         typeSkipper
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://10fastfingers.com/*
// @icon         https://www.google.com/s2/favicons?sz=64&domain=10fastfingers.com
// @grant        none
// ==/UserScript==

function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
};

( function() {
    'use strict';
     const a = document.querySelector("div#on-top").nextElementSibling.querySelector("div").innerHTML;document.querySelector("div#on-top").nextElementSibling.querySelector("div").innerHTML = `<div class="btn-group" style="margin-left:-12px; margin-right: 20px;"><a href="#" id="skip-button-yeah" class="btn btn-primary" role="button">skip</a></div>` + a

    document.querySelector("#skip-button-yeah").addEventListener("click", async (event) => {
        for (let w of word_string.split("|")) {
            w.split("").forEach((l) => {document.querySelector("input").value+=l;var e = jQuery.Event("keyup");e.which = 1;$("input").trigger(e);})
            var e = jQuery.Event("keyup");e.which = 32;$("input").trigger(e);
            await sleep(50);
        }
    })

})();

```
