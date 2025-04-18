// ==UserScript==
// @name         Orochi Prover 自动刷新与点击（延迟10秒 + 挖矿状态检测）
// @namespace    http://tampermonkey.net/
// @version      1.2
// @description  每隔30分钟刷新页面；自动通过 Cloudflare 验证；prover 按钮出现后等待10秒点击；每隔1分钟检查是否在挖矿，若未在挖矿则再次点击 prover 开始挖矿。
// @author       YourName
// @match        https://onprover.orochi.network/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    // —— 1. 每隔30分钟自动刷新页面 —— 
    setInterval(() => {
        console.log('[自动刷新] 30分钟到，重新加载页面');
        location.reload();
    }, 30 * 60 * 1000);

    // —— 2. 页面 load 完成后 —— 
    window.addEventListener('load', () => {
        // 2.1 自动勾选 Cloudflare 验证框（如果存在）
        const cfCheckbox = document.querySelector('input[type="checkbox"]');
        if (cfCheckbox) {
            console.log('[Cloudflare] 检测到勾选框，自动点击');
            cfCheckbox.click();
        }

        // 2.2 监控首次出现的 “prover” 按钮，10秒后点击开始挖矿
        const proverObserver = new MutationObserver((mutations, observer) => {
            const proverBtn = Array.from(document.querySelectorAll('button')).find(btn =>
                btn.innerText.trim().toLowerCase() === 'prover'
            );
            if (proverBtn) {
                console.log('[挖矿] 检测到 prover 按钮，10秒后开始点击');
                setTimeout(() => {
                    proverBtn.click();
                    console.log('[挖矿] 已点击 prover');
                }, 10 * 1000);
                observer.disconnect();
            }
        });
        proverObserver.observe(document.body, { childList: true, subtree: true });

        // —— 3. 每隔1分钟检查一次挖矿状态 —— 
        setInterval(() => {
            // 查看页面上是否存在 “Stop proving” 按钮
            const isMining = !!Array.from(document.querySelectorAll('button'))
                .find(btn => btn.innerText.trim().toLowerCase() === 'stop proving');
            if (!isMining) {
                // 如果不在挖矿，则尝试点击 prover 按钮重新开始
                const proverBtn = Array.from(document.querySelectorAll('button')).find(btn =>
                    btn.innerText.trim().toLowerCase() === 'prover'
                );
                if (proverBtn) {
                    console.log('[状态检测] 未发现 Stop proving，重新点击 prover');
                    proverBtn.click();
                } else {
                    console.log('[状态检测] 未发现 prover 按钮，可能页面未加载完成');
                }
            } else {
                console.log('[状态检测] 正在挖矿，状态正常');
            }
        }, 60 * 1000);
    });
})();
