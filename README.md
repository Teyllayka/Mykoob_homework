```js
// ==UserScript==
// @name         mykoob-homework
// @namespace    http://tampermonkey.net/
// @version      1.02
// @description  mykoob-homework
// @author      https://github.com/just-noname-bing https://github.com/Teyllay
// @match        https://family.mykoob.lv/?assignments/overview
// @icon         https://www.google.com/s2/favicons?domain=mykoob.lv
// @grant          GM_addStyle
// ==/UserScript==

(async function() {
    'use strict';

     function addGlobalStyle(css) {
        var head, style;
        head = document.getElementsByTagName('head')[0];
        if (!head) { return; }
        style = document.createElement('style');
        style.type = 'text/css';
        style.innerHTML = css;
        head.appendChild(style);
    }

    fillSubjects();

    addGlobalStyle('pre { overflow-x: auto; overflow-y: none;max-width: 90%; white-space: pre-wrap; white-space: -moz-pre-wrap;white-space: -pre-wrap;white-space: -o-pre-wrap; word-wrap: break-word }');
})();

 async function fillSubjects(ttable = "table#overviewList"){
    const subjects = ( await Promise.all( [ ...Object.assign(document.createElement("div"), { innerHTML: ( await ( await fetch( "https://family.mykoob.lv/?assignments/loadOverviewDataList", { credentials: "include", headers: { "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:97.0) Gecko/20100101 Firefox/97.0", Accept: "application/json, text/javascript, /; q=0.01", "Accept-Language": "en-US,en;q=0.5", "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8", "X-Requested-With": "XMLHttpRequest", "Alt-Used": "family.mykoob.lv", "Sec-Fetch-Dest": "empty", "Sec-Fetch-Mode": "cors", "Sec-Fetch-Site": "same-origin", }, referrer: "https://family.mykoob.lv/?assignments/overview", body: `start_date=${ prompt("date in (yyyy-mm-dd) format") || new Date() .toISOString() .slice(0, 19) .replace(/-/g, "-") .replace("T", " ") .split(" ")[0] }&school_classes_id=${ document.querySelector("#selectClass").value }&cls_dsc_id=-1`, method: "POST", mode: "cors", })).json()).templ, }).querySelectorAll("table#overviewList a[onclick]"), ] .map((a) => { if (!a) return; return a.attributes.onclick.value.substring( 33, a.attributes.onclick.value.indexOf(",")); }) .map(async (a) => { if (!a) return; return await ( await fetch( "https://family.mykoob.lv/?assignments/loadSingle/ow/1/id/" + a, { credentials: "include", headers: { "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:97.0) Gecko/20100101 Firefox/97.0", Accept: "application/json, text/javascript, /; q=0.01", "Accept-Language": "en-US,en;q=0.5", "X-Requested-With": "XMLHttpRequest", "Alt-Used": "family.mykoob.lv", "Sec-Fetch-Dest": "empty", "Sec-Fetch-Mode": "cors", "Sec-Fetch-Site": "same-origin", }, referrer: "https://family.mykoob.lv/?assignments/overview", method: "POST", mode: "cors", })).json(); }))) .map((a) => { if (!a) return; return a.has_files ? { ...a.task, files: (() => { let htmlFile = document.createElement("div"); htmlFile.innerHTML = a.files; return [ ...htmlFile.querySelectorAll("span[onclick]"), ].map((link) => { let sub = link.attributes.onclick.value; return ( "https://family.mykoob.lv/" + sub.substring(sub.indexOf("?")).split('"')[0]); }); })(), } : { ...a.task }; }) .map((item) => { if (!item) return; return !item.files ? { notes: item.notes, lesson: document.querySelector( `option[value='${item.subject_id}']`)?.textContent || item.subject_id, char_refdate: item.char_refdate, time: item.time, time_until: item.time_until, type_name: item.type_name, external_task_url: item.external_task_url, } : { notes: item.notes, files: item.files, lesson: document.querySelector( `option[value='${item.subject_id}']`)?.textContent || item.subject_id, char_refdate: item.char_refdate, time: item.time, time_until: item.time_until, type_name: item.type_name, external_task_url: item.external_task_url, }; });
    const table = document.querySelector(ttable);
    table.innerHTML = "";
    subjects.forEach(subject => {
        const htmlSubject = Object.assign(document.createElement("div"), {innerHTML: `<div data-subject-parent style="background-color:#edf6f9;padding:1.25rem; margin:2rem;border-radius:10px"> <h2 data-subject-lesson>LESSON</h2> <h3 data-subject-notes>NOTES</h3> <h3 data-subject-files></h3> <h3 data-subject-external_task_url></h3> <h3><span data-subject-char_refdate>DATE</span> (<span data-subject-time>TIMESTART</span> - <span data-subject-time_until>TIMEEND</span>)</h3> </div>`})
        Object.keys(subject).forEach(key => {
            const s = htmlSubject.querySelector(`[data-subject-${key}]`)
            if (!s) return;

            if(key == "files" || key == "external_task_url") return (s.innerHTML = `<a href="${subject[key] || '#'}" style="text-decoration:underline" >${subject[key] || ""}</a>`)

            s.innerHTML = subject[key]

        });
        table.innerHTML += htmlSubject.innerHTML;
    })
    }
```
