# Hugo开发日记


这篇文章记录了我的整个建站过程.
<!--more-->

# 建站记录


## 建立新博客

```
hugo new site myblog
```

## 第一次新建文章

```
hugo new posts/first-post.md

hugo -D #执行该命令生成public
```

{{< admonition tip >}}
`hugo`该命令应该==放在最后==，在一切构建都完成再进行
{{< /admonition >}}


### 本地构建bug

在**Windows**环境下使用git bash进行构建无法成功，使用powershell可以完美做到本地构建

```
hugo server -t LoveIt --buildDrafts		//在本地构建博客
```

## 部署到github

1.  在github新建仓库，并以`github.io`结尾
2.  此处使用git bash

```
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/player3up/player3up.github.io.git
git remote -v  #列出当前仓库中已配置的远程仓库，并显示它们的 URL
git remote rm origin #如果提示已存在，可使用这个命令删除
git pull --rebase origin master
git push origin master
git config --global credential.helper cache  #在其他命令开始前，避免每次输入密码
```

1.  `credential-cache`是 Git 提供的一个凭证管理器，用于在一段时间内保存用户的凭证（例如用户名和密码），以便能够在不需要再次输入凭证的情况下进行后续的操作。(如果git版本过旧无法使用该命令)
2.  且需要检查是否有与凭证管理相关的配置项

```
credential.helper=cache
```

若没有则需要手动添加

```
git config --global credential.helper cache
```

ps:使用后github部署查看时需要vpn

### 解决重复输入用户名密钥问题

在安装git时将其应用到powershell上，在第一次使用密码时会要求登录，完美解决重复问题，也不需要上述命令来保存

### git push 错误

```powershell
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/player3up/player3up.github.io.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

解决办法：

先重新拉取，再push

```
git pull --rebase origin master
git push -u origin master
```

## LoveIT主题

头像位置为相对位置，且跟配置文件有关的图像放在`themes/“你主题的名字”/static`里面，而博客文章的图像放在static里面

### 语言设置

在增加语言设置时需注意将`defaultContentLanguage = "zh-cn"`#默认中文，语句放在开头处，在末尾放置可能会导致语句无效

### 头像及网页logo

将所需图片放置在`themes/“你主题的名字”/static`路径内

通过更改根目录的`hugo.toml`文件的语句例如

```
 #  页面头部导航栏标题配置
    [params.header.title]
      # LOGO 的 URL
      logo = "/b5.png"
      # 标题名称
      name = "pb3's Blog"
      # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
      pre = ""
```

logo语句位置

```
 #  应用图标配置
  [params.app]
    # 当添加到 iOS 主屏幕或者 Android 启动器时的标题, 覆盖默认标题
    title = "我的网站"
    # 是否隐藏网站图标资源链接
    noFavicon = false
    # 更现代的 SVG 网站图标, 可替代旧的 .png 和 .ico 文件
    svgFavicon = "/b5.png"
    # Android 浏览器主题色
    themeColor = "#ffffff"
```

以及应用图标位置我这里的图片名称是`b5.png`即可更改网页图标

## 文章摘要

在编辑文章预览部分时，其分隔符和变量赋值符号发生变化

有以下几点

```
分隔符：
---
weight: 4
---
⬇（变为）
+++	（hugo自动生成）
weight: 4
+++

赋值符号由
weight: 4
⬇（变为）
weight= 4（hugo自动生成）
```

可以在博客根目录的archetypes/default.md文件中编辑文章样式，该文件会影响指令生成的文章

### 摘要图片

在设置文章摘要图片时根据hugo的[页面包](https://gohugo.io/content-management/page-bundles/)中的[页面资源](https://gohugo.io/content-management/page-resources/).来进行编辑，需将文章名字改为`index.md`、`_index.md`这样才使用resources资源进行引用


