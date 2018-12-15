安卓问题
Error:SSL peer shut down incorrectly(完美解决)
解决方案：在Android Studio中进行配置的设置，对gradle version配置文件进行更改，将其中
distributionUrl=https\://services.gradle.org/distributions/gradle-3.3-all.zip

把这个部分的网址后面的最后一个部分，改成 ** 即可

------
read time out问题
编译项目的时候，一直报这个错误，主要是gradle的问题，后来尝试了很多办法才解决
对 Android Studio -- Preferences -- HTTP Proxy
进行配置
mirrors.neusoft.edu.cm 
port：80
在网上搜到一个这个可以用，总算解决