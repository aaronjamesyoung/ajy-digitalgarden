---
{"context":"Personal","type":"Resource","status":"Active","topic":["Aaron"],"dateCreated":"2024-02-15","cssclasses":null,"dg-publish":true,"permalink":"/bookmarks/","dgPassFrontmatter":true}
---


<style>ul, ul li { list-style-type: none; padding-left: 0;}</style>
<div id="bookmarks"></div>
<script>
const BM_URL=`https://hs.ajy.co/nodered/stream/bookmarks`;
fetch(BM_URL)
.then(response=>response.text())
.then(data=>{
const elem=document.getElementById("bookmarks");
elem.innerHTML=data;
});</script>
