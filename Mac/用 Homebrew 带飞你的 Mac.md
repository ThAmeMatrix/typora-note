# 用 Homebrew 带飞你的 Mac

2017-09-15 

[ Mac](https://shockerli.net/categories/mac/)

> `Homebrew`也称`brew`，`macOS`下基于命令行的最强大软件包管理工具，使用`Ruby`语言开发。类似于`CentOS`的`yum`或者`Ubuntu`的`apt-get`，`brew`能方便的管理软件的安装、更新、卸载，省去了手动编译或拖动安装的不便，更使得软件的管理更加集中化，解决了依赖包管理的烦恼。

## 资料

- [官网](https://brew.sh/)
- [官方文档](https://docs.brew.sh/)
- [官方软件包仓库](http://formulae.brew.sh/)

## 安装

Homebrew 依赖于`Xcode Command Line Tools`，所以需先执行：

```shell
xcode-select --install
```

在终端中执行：

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

检查是否已安装成功：

```shell
$ brew -v

Homebrew 1.6.1
Homebrew/homebrew-core (git revision 0aeb7; last commit 2018-04-12)
```

## 基本用法

> 基于`brew`安装的所有软件及其依赖均会安装到目录`/usr/local/Cellar`

- **Brew 帮助信息**

  ```
  $ brew help
      
  Example usage:
    brew search [TEXT|/REGEX/]
    brew info [FORMULA...]
    brew install FORMULA...
    brew update
    brew upgrade [FORMULA...]
    brew uninstall FORMULA...
    brew list [FORMULA...]
      
  Troubleshooting:
    brew config
    brew doctor
    brew install --verbose --debug FORMULA
      
  Contributing:
    brew create [URL [--no-fetch]]
    brew edit [FORMULA...]
      
  Further help:
    brew commands
    brew help [COMMAND]
    man brew
    https://docs.brew.sh
  ```

- **子命令帮助信息** `brew help [COMMAND]`或`brew [COMMAND] -h` 用于查看具体某个子命令的帮助信息。

  例如，查看`install`命令的帮助详情：

  ```shell
  $ brew install -h
      
  brew install [--debug] [--env=(std|super)] [--ignore-dependencies|--only-dependencies] [--cc=compiler] [--build-from-source|--force-bottle] [--include-test] [--devel|--HEAD] [--keep-tmp] [--build-bottle] [--force] [--verbose] formula [options ...]:
      Install formula.
      
      formula is usually the name of the formula to install, but it can be specified
      in several different ways. See [SPECIFYING FORMULAE](#specifying-formulae).
      
      If --debug (or -d) is passed and brewing fails, open an interactive debugging
      session with access to IRB or a shell inside the temporary build directory.
      
      If --env=std is passed, use the standard build environment instead of superenv.
      
      If --env=super is passed, use superenv even if the formula specifies the
      standard build environment.
      
      If --ignore-dependencies is passed, skip installing any dependencies of
      any kind. If they are not already present, the formula will probably fail
      to install.
      
      If --only-dependencies is passed, install the dependencies with specified
      options but do not install the specified formula.
      
      If --cc=compiler is passed, attempt to compile using compiler.
      compiler should be the name of the compiler's executable, for instance
      gcc-4.2 for Apple's GCC 4.2, or gcc-4.9 for a Homebrew-provided GCC
      4.9.
      
      If --build-from-source (or -s) is passed, compile the specified formula from
      source even if a bottle is provided. Dependencies will still be installed
      from bottles if they are available.
      
      If HOMEBREW_BUILD_FROM_SOURCE is set, regardless of whether --build-from-source was
      passed, then both formula and the dependencies installed as part of this process
      are built from source even if bottles are available.
      
      If --force-bottle is passed, install from a bottle if it exists for the
      current or newest version of macOS, even if it would not normally be used
      for installation.
      
      If --include-test is passed, install testing dependencies. These are only
      needed by formulae maintainers to run brew test.
      
      If --devel is passed, and formula defines it, install the development version.
      
      If --HEAD is passed, and formula defines it, install the HEAD version,
      aka master, trunk, unstable.
      
      If --keep-tmp is passed, the temporary files created during installation
      are not deleted.
      
      If --build-bottle is passed, prepare the formula for eventual bottling
      during installation.
      
      If --force (or -f) is passed, install without checking for previously
      installed keg-only or non-migrated versions
      
      If --verbose (or -v) is passed, print the verification and postinstall steps.
      
      Installation options specific to formula may be appended to the command,
      and can be listed with brew options formula.
      
  brew install --interactive [--git] formula:
      If --interactive (or -i) is passed, download and patch formula, then
      open a shell. This allows the user to run ./configure --help and
      otherwise determine how to turn the software package into a Homebrew
      formula.
      
      If --git (or -g) is passed, Homebrew will create a Git repository, useful for
      creating patches to the software.
  ```

- **搜索软件** `brew search [TEXT|/REGEX/]` 用于搜索软件，支持使用正则表达式进行复杂的搜索。

  例如，查询静态博客生成工具`hugo`：

  ```shell
  $ brew search hugo
      
  ==> Searching local taps...
  hugo ✔
  ==> Searching taps on GitHub...
  ==> Searching blacklisted, migrated and deleted formulae...
  ```

- **查看软件信息** `brew info [FORMULA...]` 用于查询软件的详细信息。

  例如，查看软件`hugo`的详细信息：

  ```shell
  $ brew info hugo
      
  hugo: stable 0.38.2 (bottled), HEAD
  Configurable static site generator
  https://gohugo.io/
  /usr/local/Cellar/hugo/0.38.2 (32 files, 28.0MB) *
    Poured from bottle on 2018-04-19 at 10:11:29
  From: https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git/Formula/hugo.rb
  ==> Dependencies
  Build: dep ✔, go ✔
  ==> Options
  --HEAD
      Install HEAD version
  ==> Caveats
  Bash completion has been installed to:
    /usr/local/etc/bash_completion.d
  ```

  以上查询所得信息，包含了软件的最新可用版本，本机是否已安装，本机已安装的版本，安装的路径、大小、时间、Tap 源，所依赖的包，以及安装的可选项等详细信息。而这些信息可以帮助我们很方便快捷的了解如何对该软件进行相应的操作。

- **安装软件包** `brew install FORMULA...` 用于安装一个或多个软件。

  例如，安装软件`hugo`：

  ```shell
  $ brew install hugo
  ```

  安装软件命令执行之前，brew 一般会先检查更新 Homebrew 自身及 Tap 源。

- **更新软件包** `brew upgrade [FORMULA...]` 用于更新一个或多个软件，不指定软件名则更新所有软件。

- **卸载软件包** `brew uninstall FORMULA...` 用于卸载指定的一个或多个软件

  `brew uninstall --force FORMULA...` 彻底卸载指定软件，包括旧版本

- **已安装的软件列表** `brew list` 用于查询本机已安装的软件列表

- **服务管理** `brew services` 用于方便的管理 brew 安装的软件软件，类似于 Linux 下的 service 命令。

  查看`brew services`帮助信息：

  ```shell
  $ brew services
      
  brew services command:
      Integrates Homebrew formulae with macOS' launchctl manager.
      
      [sudo] brew services list
      List all running services for the current user (or root)
      
      [sudo] brew services run formula|--all
      Run the service formula without starting at login (or boot).
      
      [sudo] brew services start formula|--all
      Start the service formula immediately and register it to launch at login (or boot).
      
      [sudo] brew services stop formula|--all
      Stop the service formula immediately and unregister it from launching at login (or boot).
      
      [sudo] brew services restart formula|--all
      Stop (if necessary) and start the service immediately and register it to launch at login (or boot).
      
      [sudo] brew services cleanup
      Remove all unused services.
      
      If sudo is passed, operate on /Library/LaunchDaemons (started at boot).
      Otherwise, operate on ~/Library/LaunchAgents (started at login).
  ```

  查询服务列表：

  ```shell
  $ brew services list
      
  Name      Status  User    Plist
  redis     stopped
  php56     started shocker /usr/local/opt/php56/homebrew.mxcl.php56.plist
  mongodb   started shocker /usr/local/opt/mongodb/homebrew.mxcl.mongodb.plist
  memcached stopped
  httpd     stopped
  nginx     started shocker /usr/local/opt/nginx/homebrew.mxcl.nginx.plist
  etcd      stopped
  mysql@5.6 started shocker /usr/local/opt/mysql@5.6/homebrew.mxcl.mysql@5.6.plist
  ```

- **检查可更新的软件列表** `brew outdated` 可查询有更新版本的软件

  ```shell
  brew outdated
  wget (1.18) < 1.19.4_1
  redis (3.2.8) < 4.0.9
  brotli (1.0.3) < 1.0.4
  glib (2.56.0) < 2.56.1
  etcd (3.2.6) < 3.3.3
  tmux (2.3_2) < 2.6
  openssl@1.1 (1.1.0e) < 1.1.0h
  mysql@5.6 (5.6.34) < 5.6.39
  libzip (1.5.0) < 1.5.1
  bash-completion (1.3_1) < 1.3_3
  ```

- **清理软件** `brew cleanup -n` 列出需要清理的内容 `brew cleanup` 清理所有的过时软件 `brew cleanup [FORMULA]` 清理指定软件的过时包

  ```shell
  $ brew cleanup
      
  Removing: /usr/local/Cellar/wget/1.18... (9 files, 1.6MB)
  Removing: /usr/local/Cellar/libxml2/2.9.4_2... (277 files, 9.8MB)
  Warning: Skipping redis: most recent version 4.0.9 not installed
  Warning: Skipping brotli: most recent version 1.0.4 not installed
  Removing: /usr/local/Cellar/icu4c/60.1... (249 files, 66.9MB)
  Removing: /usr/local/Cellar/icu4c/60.2... (249 files, 67MB)
  Warning: Skipping glib: most recent version 2.56.1 not installed
  Removing: /usr/local/Cellar/readline/7.0... (45 files, 2MB)
  Removing: /usr/local/Cellar/unixodbc/2.3.4... (39 files, 952.3KB)
  Removing: /Users/shocker/Library/Logs/Homebrew/go... (64B)
  Removing: /Users/shocker/Library/Logs/Homebrew/glide... (64B)
  ...
  ==> This operation has freed approximately 214.1MB of disk space.
  ```

- **查看配置信息** `brew config` 用于查看 brew 所在环境及相关的配置情况

  ```shell
  $ brew config
      
  HOMEBREW_VERSION: 1.6.1
  ORIGIN: https://github.com/Homebrew/brew.git
  HEAD: 5a2817cb023ca5e929fe030d423002992bdabf1b
  Last commit: 7 days ago
  Core tap ORIGIN: https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
  Core tap HEAD: 0aeb738f9f10ba3edd4edda9551fe11441bec458
  Core tap last commit: 11 days ago
  HOMEBREW_PREFIX: /usr/local
  HOMEBREW_BOTTLE_DOMAIN: https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles
  CPU: quad-core 64-bit haswell
  Homebrew Ruby: 2.3.3 => /usr/local/Homebrew/Library/Homebrew/vendor/portable-ruby/2.3.3/bin/ruby
  Clang: 9.1 build 902
  Git: 2.15.1 => /Library/Developer/CommandLineTools/usr/bin/git
  Curl: 7.54.0 => /usr/bin/curl
  Java: 9.0.4
  macOS: 10.13.4-x86_64
  CLT: 9.3.0.0.1.1521514116
  Xcode: N/A
  XQuartz: N/A
  ```

- **诊断问题** `brew doctor` 诊断当前 brew 存在哪些问题，并给出解决方案

- **仓库管理** `brew tap` 已安装的仓库列表

  ```shell
  $ brew tap
      
  Updating Homebrew...
  caskroom/cask
  homebrew/core
  homebrew/services
  peckrob/php
  phinze/cask
  ```

  `brew tap [--full] user/repo [URL]` 添加仓库

  `brew untap tap` 移除仓库

## 源镜像

> 如果不用源镜像，那么就自带梯子或自建服务吧~

- 清华大学开源软件镜像站

  > [Homebrew 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/)
  >
  > [Homebrew-bottles 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew-bottles/)

- 中科大 LUG@USTC

  > [替换及重置Homebrew默认源](https://lug.ustc.edu.cn/wiki/mirrors/help/brew.git)
  >
  > [Homebrew Bottles源](https://lug.ustc.edu.cn/wiki/mirrors/help/homebrew-bottles)

- ban.ninja

  > [http://ban.ninja](http://ban.ninja/)
  >
  > 该源只镜像了二进制预编译包，即 Bottles 源

文章作者 Jioby

上次更新 2018-04-23

许可协议 [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

原文链接 https://shockerli.net/post/macos-homebrew-manual/

[Mac](https://shockerli.net/tags/mac/) [brew](https://shockerli.net/tags/brew/)

[ sysstat——系统性能监控神器](https://shockerli.net/post/linux-tool-sysstat/)[PHP 实现斐波那契数列的三种方式 ](https://shockerli.net/post/fibonacci-sequence-implement-by-php/)



© 2019 格物