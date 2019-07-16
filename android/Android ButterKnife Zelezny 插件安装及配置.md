# Android ButterKnife Zelezny 插件安装及配置

原文： https://blog.csdn.net/qq_34476727/article/details/68958291



ButterKnife属于更新比较频繁的框架，注解型框架，从6.1.0到8.0.1版本，作者把inject()方法改成了bind()方法，@Bind(R.id.button)改成了@BindView(R.id.button)。

**操作方法：**

1、安装ButterKnife框架

在线安装：点击File→Project Structure→Modules下对应的项目→Dependencies→右侧的加号→Library Dependency→输入ButterKnife搜索，点击下载com.jakewharton:butterknife:7.0.1

2、安装Android ButterKnife Zelezny插件

点击File→Settings→Plugins→Browse repositories→输入Android ButterKnife Zelezny→点击Install安装并重启

3、重启Android Studio

然后点击Sync Now

4、打开Activity的class文件，鼠标放在R.layout.activity_main上点击鼠标右键→Generate即可看到Generate ButterKnife Injections

