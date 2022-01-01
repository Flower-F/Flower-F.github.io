---
title: 最后还是 nest.js 吗
date: 2021-12-10 22:50:33
tags: [nest.js, 闲聊]
copyright: true
---
思考和尝试了很久 node 做后端怎样才更方便快捷。首先现在开发肯定要首先 typescript，然后框架方面选择基本也就是 express、koa、egg.js、nest.js、midway.js。一开始我的选择是 nest.js，因为它天然支持 typescript，而且封装程度较高，使用也很方便。但是感觉自己如果要去了解它里面的原理的话难度比较大。出于想要了解原理的目的，我又转向了 koa，因为 koa 的代码是出了名的简短精悍。然后在一晚上的尝试以后， sequelize-typescript 出现了一个不知道缘由的 bug，表无法自动创建。然后我又换成了 typeorm，typeorm 使用起来真的是非常方便。不过这一整套操作下来，安装各种包以及相对应的 @types 包，我又开始怀念起来 nest.js 那种开箱即用的感觉了。尤其是它的命令行配置，以及跟 class-validator 的完美融合使用起来确实是太方便了。总而言之，反反复复思考了很久，最后决定下来了，下次做项目后端还是用 nest.js 吧。