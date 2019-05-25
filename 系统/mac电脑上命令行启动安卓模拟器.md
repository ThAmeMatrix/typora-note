# mac电脑上命令行启动安卓模拟器

来源: https://juejin.im/post/5bcfe1e7518825779a41fa5e

> 假设你已经配置好的安卓环境,如果你是做`ReactNative`开发者,使用`android studio`编辑器中自带模拟器,现在介绍如何在`mac`电脑上配置命令行启动模拟器



### 一、启动的步骤

- 1、在`.bash_profile`环境变量中配置

  ```shell
  export ANDROID_SDK_ROOT=/Users/[username]/Library/Android/sdk #[username]是你电脑名
  export PATH=$ANDROID_SDK_ROOT/emulator:$ANDROID_SDK_ROOT/tools:$PATH
  ```

- 2、使用命令查看本机中带的模拟器

  ```shell
  emulator -list-avds
  # Pixel_2_API_28(我本机上模拟器)
  ```

- 3、进入`tools`文件夹下新增`startAndroidEmulator.sh`文件

  ```shell
  /Users/[username]/Library/Android/sdk/tools #[username]是你电脑名
  ```

- 4、`startAndroidEmulator.sh`文件内容

  ```shell
  pushd ${ANDROID_HOME}/tools
  emulator -avd Pixel_2_API_28
  popd
  ```

- 5、刷新环境变量

  ```shell
  source .bash_profile
  ```

- 6、运行命令启动模拟器

  ```shell
  startAndroidEmulator.sh
  ```

- 7、如果遇到没权限执行的时候

  ```shell
  chmod +x 文件
  ```