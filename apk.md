# Tools
[dex2jar](https://mac.softpedia.com/get/Developer-Tools/dex2jar.shtml)将dex文件转换成Jar包 

[jd-gui](https://mac.softpedia.com/get/Development/Java/JD-GUI.shtml) 将Jar包文件反编译成java源文件 

[apktool](https://mac.softpedia.com/get/Developer-Tools/Apktool.shtml) 查看二进制文件 

# description
1 apk文件的本质是压缩文件，我们将apk文件修改后缀名为zip或者rar等，可以直接解压缩查看apk文件夹
2 使用dex2jar将*.dex文件转换成Jar包--sh d2j-dex2jar.sh -f ../../jhs.apk
3 使用jd-gui将Jar包文件反编译成java源文件
4 jd-gui下载解压后，直接打开文件夹里面的JD-GUI，即可打开图形化界面。将我们上一个步骤生成的classes-dex2jar.jar直接拖动进入界面中，就可以看到反编译之后的源码结构了
5 使用apktool工具查看apk里的二进制文件下载下来apktool并解压缩后，调用java命令执行--java -jar apktool.jar d yourApkName.apk
# 参考
[Apk 反编译实践](https://www.jianshu.com/p/9e0d1c3e342e)
