---
layout:     post
title:      "🤖️[Expired] The Self-hosted OpenAI ChatGPT API"
subtitle:   "💰Free of Charge in March."
date:       2023-03-05 23:59:00
author:     "Shicong Liu"
header-img: "img/home-dark.jpg"
catalog: true
mathjax: true
comment: true
hide-in-nav: true
tags:
    - 日常
    - feed
---



Recently OpenAI have released the API of ChatGPT and charges it for 0.002 dollars per 1000 query tokens[^1]. An $18 check has been given to the users for trial, and I therefore registered an api-key to make full use of it. 



Thanks to the front-end engineers, an UI was given open-sourced[^2] to make it easier to deploy on the website. I have bought a vultr VPS and deployed it at [this website](http://63.211.111.102:5000/). Feel free to chat with it since the given $18 check is far beyond individual consumption.



Just try it.

`Mar. 5`: Improved the *temperature* parameter to increase the diversity of answers.



> Note that since some pre-defined prompts are in Chinese, the bot is more likely to use Mandarin, but is still able to speak other languages with proper queries. This deployment actually plays a role in avoiding some proxy censorships.



---

[^1]:[OpenAI Blog](https://openai.com/blog/planning-for-agi-and-beyond)
[^2]:[ChatGPT Web repo](https://github.com/869413421/chatgpt-web)
