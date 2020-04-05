---
layout:     post
title:      解决使用Jekyll和GitHub Page博客构建错误
subtitle:   Unable to build page. Please try again later.
date:       2020-04-05 
author:     terese
header-img: img/post-12.jpg
catalog:   true
tags:
    - 博客
---

使用Jekyll+GitHub Page搭建博客遇到出现404页面或者pull代码后博客却没有更新这种情况，先查看邮箱是否出现错误提示：

如果出现错误消息提示，根据报错进行修改：

![image-20200405140731977](https://tva1.sinaimg.cn/large/00831rSTgy1gdiuxtyo8zj30o0050q3o.jpg)

如果收到下面这种错误说明你需要自己检查错误：

```
The page build failed for the `master` branch with the following error:

Unable to build page. Please try again later.
```

>[官方给的建议](https://help.github.com/en/github/working-with-github-pages/troubleshooting-jekyll-build-errors-for-github-pages-sites)
>
>如果收到一般错误消息，请检查常见问题。
>
>- 您正在使用不受支持的插件。有关更多信息，请参见“ [关于GitHub Pages和Jekyll”](https://help.github.com/en/articles/about-github-pages-and-jekyll#plugins)。
>- 您的存储库超出了我们的存储库大小限制。有关更多信息，请参阅“ [我的磁盘配额是多少？](https://help.github.com/en/articles/what-is-my-disk-quota) ”
>- 您更改了*_config.yml*文件中的`source`设置。GitHub Pages在构建过程中会覆盖此设置。
>- 发布源中的文件名包含`:`不支持的冒号（）。
>
>如果您收到特定的错误消息，请查看下面的错误消息的疑难解答信息。
>
>修复所有错误之后，将更改推送到站点的发布源，以触发GitHub上的另一个构建。

拿我的🌰说明，我是因为更新的一篇博客里有个代码部分出现报错

```
The tag `load` on line 82 in `_posts/2019-05-27-Django项目实践笔记1.md` is not a recognized Liquid tag. 
```

如果找不到错误在哪里，最简单的方法就是回退到博客构建成功的历史版本，回退前请注意备份你修改后的文件，因为本地仓库将同时更新：

步骤：

1. **查找 commit id：**浏览GitHub上的提交历史记录，找到要回退的版本，复制commit id。

   Github Desktop中：

   ![image-20200405141339778](https://tva1.sinaimg.cn/large/00831rSTgy1gdiv459uchj30b402zt8o.jpg)

   Github中：

   ![image-20200405141646045](https://tva1.sinaimg.cn/large/00831rSTgy1gdiv7dw5uuj30mi060dg3.jpg)

2. 恢复历史版本

   `git reset --hard [你的commit id]`

3. 推送到GitHub远程仓库

   `git push -f -u origin master `

4. 回退到成功构建的博客版本后，不要大批量提交你的更新文件，尝试分批提交，提交后关注邮件是否有错误提示。通过这种方式能够找出错误的文件。找到错误文件后，单独提交该文件能够收到邮件准确的位置报错，但是批量提交不能收到，我前面一次性更新了10个文件所以没有准确的位置报错，只收到**Unable to build page**。

还有一种方式是：

在本地调试，这种方式长久来看更方便。

官方说明：[使用Jekyll在本地测试GitHub Pages网站](https://help.github.com/en/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll)

步骤：（Mac系统）

1. brew 安装ruby

   ```
   brew update
   brew install ruby
   ruby --version#确认已成功安装了最新版本的 Ruby
   ```

2. ```
   gem install bundler
   ```

3. 安装 `jekyll`和 `jekyll bundler`

   ```ruby
   $ gem install jekyll
   $ gem install jekyll bundler
   ```

4. 进入你的 **Blog 所在目录**，然后创建本地服务器

   ```ruby
   $ jekyll s
   ```

5. 在 [http://127.0.0.1:4000/](https://links.jianshu.com/go?to=http%3A%2F%2F127.0.0.1%3A4000%2F) 可以看到你的博客，本地就能测试博客啦～

