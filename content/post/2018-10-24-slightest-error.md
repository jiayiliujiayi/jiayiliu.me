---
title: 不仅是大小写敏感的问题：差之毫厘谬之千里
author: Jiayi
date: '2018-10-24'
slug: slightest-error
categories:
  - code
tags:
  - reflection
---

前天想在主题栏多加两个个选项（eg. blog，about，weibo），就更改了config文件，也没太在意细节，直接按照github的简介来着；而后又加了一篇博文。commit和push都没问题。最终，无论如何也无法deploy。推测了几个可能：检查了variable里的hugo version，重新开始一个deployment，删除了可疑的最新博文，但还是部署失败。  

问题一直持续到今天（昨天很鸵鸟，借着饱暖思淫欲的借口）。试着deploy了local branch，成功，再试master branch，失败。结论：master branch很大可能有问题。  

再从屡次commit里逐一查找差别。最终找到一个最最可疑的内容： 
<img src="/post/2018-10-24-slightest-error_files/Screen Shot 2018-10-25 at 12.05.29.png" alt="nozuonodie" width="80%"/>  

修改回“basics”后，deploy成功。  

总结  
1. 不要随便改config文件  
2. 在大小写敏感环境里不要随便改大小写  
3. 把大问题分成小问题，按照时间顺序来  
4. commit note写得要详细一些，这样troubleshooting的方向更明确 & 效率更高  

近期在coding方面想学的东西：  
0. rmd的静态文件怎么call  
1. 修改主题的格式    
2. 定义网页动态标题（不知道是不是这么说），参见[柳志超的blog](https://liuzhichao.com/2018/hello_hugo/)，浏览该网页时是正常标题，浏览其他网页时标题变为“I miss you ”太酷了！  
3. 改字体

---------------------------------10月25日update-------------------------  
今日再次“作死”：把某一篇的title改了，导致无法deploy，具体原因还不知道，但似乎是和index有关- -  
--> 尝试改中文的title就没有关系 但是改英文title就会出现无法部署的情况。。。  
--> 改了某个英文标题，以及改了public下面的index.html和index.xml文件，deploy失败。。  
to be continued  

------------------------------接下来是五十步笑百步时间---------------------------------------  
家父几年前的博文：    
![](/post/2018-10-24-slightest-error_files/original_Q50f_2159000000f1118f.jpg)  
如今我居然能看懂（其实是因为自己也遇到过一模一样的问题哈哈）
