# SA-MP Mobile

[Chinese edition](https://github.com/SA-MP-Mobile/SA-MP-Mobile/blob/master/README-CN.MD)

Forked from [SAMP-Android](https://github.com/y-kalcor/SA-MP-Android "SAMP-Android")

## Getting Started

### Statement

- 1.This the oldest source of SA-MP Mobile,we won't open source the latest version,this repository is aim to help you solve the basic problem when you want to run it.

- 1.I have tested the steps on Windows 10,it can also run on MAC and Linux,but some details may be different.

- 2.The word "rootdir" refers to the source's absolute path (e.g:C:\SA-MP\Mobile).

- 3.I'm not responsible for the problems caused by this repository,I don't provide the service for you to solve the problem.

- 4.You can make the secondary development but you must keep the copyright of the origin developer.

Contact:

- Tencent QQ 1270329917
- Mail mkowes@vip.qq.com or hobrimttxx@gmail.com

Thanks:

- IGTA Tuise

### Precautions

- 1.This version only support English,Russian.If you want to support more language,use the iconv to convert the message buffer from the origin coding to UTF-8.

- 2.This version don't support Textdraw,Object,some types of Dialogs and so on.

- 3.If you game exits as soon as you run the app,you need to check you config files is put in the appropriate dir or not.

## Requirements

### Environment

- [Source](https://github.com/SA-MP-Mobile/SA-MP-Mobile/archive/build69.zip)
- [Android-NDK r14b](https://developer.android.com/ndk/downloads/older_releases.html)
- [JDK](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
- [ApkTool](https://ibotpeaches.github.io/Apktool/install/)

### System

```bash
> cd rootdir
> git clone https://github.com/SA-MP-Mobile/SA-MP-Mobile.git
```

Edit the user Enviromnet Variable 'Path' and add these two dirs:1."NDK-CMD.bat"'s dir 2.jdk/bin 3.rootdir\files

## 1.Compilation

Open the cmd or PowerShell.

```bash
> cd rootdir\jni
> NDK_PROJECT_PATH=rootdir NDK_DEBUG=0 ndk-build.cmd -B
```

## 2.Unpack the Game

```bash
> cd rootdir\files
> .\apktool d rootdir\files\build69.apk -o rootdir\files\apk -f
```

## 3.Pack the game

Paster rootdir\libs\armeabi-v7a\libsamp.so to rootdir\files\lib\armeabi-v7a

then

```bash
> cd rootdir\files
> .\apktool b rootdir\files\apk -o rootdir\files\apk\dist\build69.apk -f
```

## 4.Sign the apk

You can search this on Google or Baidu.

### 4.1.Generate the signature

```bash
#> chcp 936 # If you are Chinese(use the GBK coding,run this at first)
> cd rootdir\files
> keytool -genkey -v -keystore sampandroid.jks -keyalg RSA -keysize 2048 -validity 10000 -alias sampandroid
# Follow the notice and fill the blank.
> keytool -importkeystore -srckeystore sampandroid.jks -destkeystore sampandroid.jks -deststoretype pkcs12
```

### 4.2.Sign

```bash
> cd rootdir\files\apk\dist
> jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore rootdir\files\sampandroid.jks build69.apk sampandroid
```

Then you can install rootdir\files\apk\dist\build69.apk in your Android.

## 5.Install

If you have installed the GTA:SA,remove it!But remember to make the backup of the data.

Install rootdir\files\apk\dist\build69.apk

Restore the data.

Copy rootdir\data to the root dir of the game.

## 6.Setting

Edit Android\obb\com.rockstargames.gtasa\files\SAMP\settings.ini.

## 7.Play

Open the app,click the second option of the main interface.
