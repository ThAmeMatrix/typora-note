# 你应该定期更新 Homebrew

原文：https://segmentfault.com/a/1190000004353419

# TL;DR

这篇文章是关于定期更新 Homebrew 的话题。它会告诉你定期更新的好处，常用的命令，以及用 `brew pin`尽可能无痛地更新。

# 为什么要定期更新

我发现不少人都不会经常更新，或者只在必须用某个工具的新版本的时候才更新。他们的看法是，更新有可能产生一些意外的问题，反正当前环境足够稳定可以用，干嘛自找麻烦呢？

这个看法对也不对。对是因为，更新产生的潜在问题不可避免。不对是因为总有一天你需要升级的，也许是为了某个工具的新特性，也许是为了修复软件的漏洞，也许你安装的包非要依赖另一个包的新版本，等等。如果隔了很长一段时间才升级，那潜在的小问题可能就会变成大问题。

另一个有意思的现象是，当碰到比较破坏性的事情，比如 Mac OS 大版本更新后，很多人会选择重装 Homebrew 然后顺带安装最新版的包。很少人会去装一个指定的旧版本（除了特殊项目需要）。这说明他们不是不想用新版本，而是不想痛苦地更新。

既然总有一天需要更新，而更新带来问题不可避免，那为什么不更新得频繁点呢？这个道理跟 Git 的冲突解决有相似性。长时间不 pull/push 的代码更容易产生冲突，一个解决方法就是频繁地 commit & merge 。

我现在试着一个月更新一次，两次下来发现这些好处：

1. 每次更新的包很少，更新风险也小。
2. 更容易发现不需要的包，便于清理，不为不需要的东西买单。
3. 定期清理旧版本，释放空间。

更新流程其实都差不多，下面列一下我常用的命令。

# 更新 Homebrew

要获取最新的包的列表，首先得更新 Homebrew 自己。这可以用 `brew update` 办到。

```shell
brew update
```

完后会显示可以更新的包列表，其中打钩的是已经安装的包。输出类似下面这样：

```shell
Updated Homebrew from fe93aa3 to 6ae64c3.
Updated 1 tap (homebrew/versions).
==> Updated Formulae
awscli      cmake ✔     homebrew/versions/libmongoclient-legacy
```

# 更新包 (formula)

更新之前，我会用 `brew outdated` 查看哪些包可以更新。

```shell
brew outdated
```

然后就可以用 `brew upgrade` 去更新了。Homebrew 会安装新版本的包，但旧版本仍然会保留。

```shell
brew upgrade             # 更新所有的包
brew upgrade $FORMULA    # 更新指定的包
```

# 清理旧版本

一般情况下，新版本安装了，旧版本就不需要了。我会用 `brew cleanup` 清理旧版本和缓存文件。Homebrew 只会清除比当前安装的包更老的版本，所以不用担心有些包没更新但被删了。

```shell
brew cleanup             # 清理所有包的旧版本
brew cleanup $FORMULA    # 清理指定包的旧版本
brew cleanup -n          # 查看可清理的旧版本包，不执行实际操作
```

这样一套下来，该更新的都更新了，旧版本也被清理了。

# 锁定不想更新的包

如果经常更新的话，`brew update` 一次更新所有的包是非常方便的。但我们有时候会担心自动升级把一些不希望更新的包更新了。数据库就属于这一类，尤其是 PostgreSQL 跨 minor 版本升级都要迁移数据库的。我们更希望找个时间单独处理它。这时可用 `brew pin` 去锁定这个包，然后 `brew update` 就会略过它了。

```shell
brew pin $FORMULA      # 锁定某个包
brew unpin $FORMULA    # 取消锁定
```

# 其他几个常用命令

`brew info` 可以查看包的相关信息，最有用的应该是包依赖和相应的命令。比如 Nginx 会提醒你怎么加 `launchctl` ，PostgreSQL 会告诉你如何迁移数据库。这些信息会在包安装完成后自动显示，如果忘了的话可以用这个命令很方便地查看。

```shell
brew info $FORMULA    # 显示某个包的信息
brew info             # 显示安装了包数量，文件数量，和总占用空间
```

`brew deps` 可以显示包的依赖关系，我常用它来查看已安装的包的依赖，然后判断哪些包是可以安全删除的。

```shell
brew deps --installed --tree # 查看已安装的包的依赖，树形显示
```

输出如下：

```shell
elixir (required dependencies)
└── :erlang

wxmac (required dependencies)
├── jpeg
├── libpng
│   └── xz
└── libtiff
    └── jpeg
```

还有很多有用的命令和参数，没事 `man brew` 一下可以涨不少知识。

# 小结

不想更新 Homebrew 往往有两个原因，害怕潜在的风险和对工具的不熟悉，我之前也是这样。写这篇文章最开始是为了帮我记录常用的命令方便以后查阅的。希望它也能帮到你。

# 参考资料

[Keeping Your Homebrew Up to Date](https://www.safaribooksonline.com/blog/2014/03/18/keeping-homebrew-date/)
[Homebrew FAQ](https://github.com/Homebrew/homebrew/blob/master/share/doc/homebrew/FAQ.md)