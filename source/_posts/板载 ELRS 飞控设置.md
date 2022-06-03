---
title: 板载 ELRS 飞控设置
top: true
categories:
  - FPV
tags:
  - FPV
  - ELRS
date: 2022-06-03 00:00:00
cover: https://ghproxy.com/https://raw.githubusercontent.com/ExpressLRS/ExpressLRS-Hardware/master/img/banner.png
---

<div class=md-content data-md-component=content>
    <article class="md-content__inner md-typeset">
        <h1 style="text-align: center">板载 ELRS 飞控设置</h1>
        <h2>绑定密码</h2>
        <div class=bp-wrapper> <input class="md-input bp-input" type=text placeholder=...> </div>
        <p>UID Bytes
        <div class=highlight>
            <pre><code></code></pre>
        </div>
        </p>
        <h4>设置绑定密码</h4>
        <p>在Betaflight CLI命令行里面输入以下命令（现在上面输入你的密码）</p>
        <div class=highlight>
            <pre><code></code></pre>
        </div>
        </p>
        <h2 id=rf-mode-adjustment>射频模式</h2>
        <p>
            板载 ExpressLRS SPI 接收器的 AIO 飞控默认设置<code>500Hz</code>。</br>要调整它，你需要进入 Betaflight CLI 并使用以下命令：
        </p>
        <div class="highlight">
            <pre><code>set expresslrs_rate_index = [序号]</br>save</code></pre>
        </div>
        <p>
            上面的<code>[序号]</code>替换成以下内容：
            <li>500Hz = 0</li>
            <li>250Hz = 1</li>
            <li>150Hz = 2</li>
            <li>50Hz = 3</li>
        </p>
    </article>
    <script type=text/javascript>
        window.addEventListener("load", (event) => {
            initBindingPhraseGen();
        });
    </script>
    <script type=text/javascript>
        function getBytesFromWordArray(wordArray) {
            const result = [];
            result.push(wordArray.words[0] >>> 24);
            result.push((wordArray.words[0] >>> 16) & 0xff);
            result.push((wordArray.words[0] >>> 8) & 0xff);
            result.push(wordArray.words[0] & 0xff);
            result.push(wordArray.words[1] >>> 24);
            result.push((wordArray.words[1] >>> 16) & 0xff);
            return result;
        }
        function uidBytesFromText(text) {
        const bindingPhraseFull = `-DMY_BINDING_PHRASE="${text}"`;
        const bindingPhraseFullEncoded = CryptoJS.enc.Utf8.parse(bindingPhraseFull);
        const bindingPhraseHashed = CryptoJS.MD5(bindingPhraseFullEncoded);
        const uidBytes = getBytesFromWordArray(bindingPhraseHashed);
        return uidBytes;
        }
        function initBindingPhraseGen() {
        const codeTags = document.getElementsByTagName("code");
        const codeTagsArr = [...codeTags];
        const emptyCodeTags = codeTagsArr.filter((codeTag) => {
            return codeTag.innerText.trim() === "";
        });
        if (emptyCodeTags.length !== 2) {
            return;
        }
        const [output, bfOutput] = emptyCodeTags;
        output.textContent = "";
        function setOutput(text) {
            const uidBytes = uidBytesFromText(text);
            output.textContent = uidBytes;
            bfOutput.textContent = `set expresslrs_uid = ${uidBytes}\nsave`;
        }
        function updateValue(e) {
            setOutput(e.target.value);
        }
        const input = document.querySelector(".bp-input");
        if (!input) {
            return;
        }
        input.addEventListener("input", updateValue);
        setOutput("");
        }
        </script>
        <script type=text/javascript src="https://cdn.bootcdn.net/ajax/libs/crypto-js/4.1.1/crypto-js.js"></script>
</div>
