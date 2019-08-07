---
title: 在Jekyll中建立多个目录
author: Chunyu
layout: post
categories: [questions]
---
某天心血来潮我想在github上建立一个个人主页，我进入http://jekyllthemes.org/ fork了一份模板。该模板提供了建立主页、写博客和建立新的页面（例如写一份读书清单）的功能，左方的菜单页可任意添加新的目录，要发布的文章要按照日期-名字的格式放入_posts文件中。

我想要实现的功能是将文章分类，归到不同目录下面。

为了解决这个问题：  
1.我先在菜单页添加我想要的目录，目录里的layout填第二步中创建的文件的名字；  
2.再在_layouts文件夹中创建以目录名命名的html文件，每个文件里都复制粘贴进blog模板的代码，再将原来代码里的assign _posts = site.post换为assign _posts = site.categories.目录名；  
3.最后在每篇文章里添加了categories，同一类文章的categories相同（为目录名）。

其中最重要的步骤是第二步，原来的代码的意思是_posts文件中的所有文章都是该模板的对象，改过的代码将范围缩小，以categories的名字为区分，将所有文章分为好几类，且对应不同的目录。