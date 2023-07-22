---
title: 首页
comments: false
---
# 首页

!!! abstract "[一言](https://hitokoto.cn/)"
    <br/>
    <br/>
    <div id="hitokoto">
        <div id="hitokoto_text" style="text-align: center; font-size: 2.0em;">这里该写点什么呢</div>
        <br/>
        <div id="hitokoto_author" style="text-align: right; font-size: 1.8em;"></div>
    </div>
    <br/>
    <br/>

<script>
    fetch("https://v1.hitokoto.cn", {
        method: "GET",
    }).then(res => res.json()).then(res => {
        let hitokoto_text = document.getElementById("hitokoto_text");
        console.log(res.uuid);
        hitokoto_text.innerHTML = `<a href="https://hitokoto.cn/?uuid=` + res.uuid + `" style="color: inherit" target="_blank"> 『 ` + res.hitokoto + ` 』 </a>`;
        let hitokoto_author = document.getElementById("hitokoto_author");
        hitokoto_author.innerHTML = "—— " + res.from;
    }).catch(err => {
        console.log(err);
    });
</script>