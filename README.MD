# SAMP-Android

Forked from [SAMP-Android](https://github.com/y-kalcor/SA-MP-Android "SAMP-Android")

## 零.声明

### 1.个人声明

- 1.本文全程均已在Windows 10 x64上测试过，其他系统理论上可以，但部分细节处步骤可能不一致，请自行解决

- 2.文中“根目录”一词指本库所在文件夹的绝对路径（如：C:\SAMP-Android）

- 2.本人不为本项目衍生的任何问题负责

- 3.任何人均可使用本文提供的方法，任何人均可进行二次开发和发行，但不可以删除原作者的版权

- 4.本人不提供任何有偿或无偿技术支持（说明白了就是别来烦我，别问我乱七八糟的问题，除非写的有问题）

有特殊情况请联系本人 QQ1270329917

特别感谢：IGTA-褪色

### 2.注意事项

- 1.目前不支持中文。中文默认会被转码到俄语或乱码 咋搞呢？用iconv将消息流从UTF8编码转到GBK即可，自己开发去吧

- 2.绝大多数服务器都玩不成！别妄图直接拿去开快餐

- 3.自带的虚拟键盘只支持英文、数字、俄语，不要妄想搞内置中文输入法了（你能一个人造出一套中文输入法，还用来搞samp？），想支持中文？自己想办法（IGTA的方案是外置一个软件）

- 4.打开游戏后闪退？配置文件应该放错位置了，自己研究自己的手机里应该放哪吧 或者你打包/签名出问题了！

## 一.准备环境

### 1.需要下载的东西

- [这个库](https://github.com/Hobr/SA-MP-Android/archive/master.zip) 不用解释
- [Android-NDK r14b](https://developer.android.com/ndk/downloads/older_releases.html) 编译
- [JDK](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 对重新打包的apk进行签名

- 附加：游戏数据包 [提取码: g4fp](https://pan.baidu.com/s/1hzLMiT-mOsRTtZxjH8eUzQ)

### 2.系统环境配置

将本库放到一个固定的位置“根目录”

在环境变量“Path”下分别新增：1.NDK所在目录 2.jdk所在目录下的bin目录 3.根目录\files

## 二.编译

打开Powershell或者cmd

```bash
> cd 根目录\jni
> ndk-build.cmd NDK_PROJECT_PATH=根目录 -B NDK_DEBUG=0
```

## 三.解包

```bash
> cd 根目录\files
> .\apktool d 根目录\files\build69.apk -o 根目录\files\apk -f
```

## 四.打包

先将 根目录\libs\armeabi-v7a\libsamp.so 替换到 根目录\files\lib\armeabi-v7a 目录下（要不然你搞了半天等于啥也没做......）

```bash
> cd 根目录\files
> .\apktool b 根目录\files\apk -o 根目录\files\apk\dist\build69.apk -f
```

## 五.签名

网上有很多本部分的相关资料，可以参照其他地方的方法

### 1.生成签名

```bash
> chcp 936 # Console GBK（也可以不执行，但部分情况下JDK输出可能是乱码）
> cd 根目录\files
> keytool -genkey -v -keystore sampandroid.jks -keyalg RSA -keysize 2048 -validity 10000 -alias sampandroid
# 接下来请按照提示输入相关信息
> keytool -importkeystore -srckeystore sampandroid.jks -destkeystore sampandroid.jks -deststoretype pkcs12
```

### 2.签名

```bash
> cd 根目录\files\apk\dist
> jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore 根目录\files\sampandroid.jks build69.apk sampandroid
```

根目录\files\apk\dist\build69.apk 为最终可安装版本

## 六.手机上的准备

删除原来的GTASA（如有数据包，请提前备份）

安装 根目录\files\apk\dist\build69.apk

恢复数据包

复制 根目录\data 目录内的目录到安卓跟目录（大多数手机是这样操作的，部分手机操作不一致）

## 七.设置

在手机中打开 Android\obb\com.rockstargames.gtasa\files\SAMP\settings.ini，修改[client]段相关信息（例如服务器ip 游戏昵称）

## 八.愉快的玩♂耍♂

打开手机里的GTA:SA，进入主界面后第二个按键为Multiplayer，点击即可

## 奔溃调试

```bash
> adb logcat | ndk-stack -sym 根目录\obj\local\armeabi-v7a
```

