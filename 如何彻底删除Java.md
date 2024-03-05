# 来源

本内容大部分取自腾讯云开发者社区，作者名`全栈程序员站长`，[文章链接](https://cloud.tencent.com/developer/article/2103091)

# 删除步骤

1. 点击控制面板—>程序—>卸载程序，卸载掉JDK和JRE，即Java8和Java SE，如图：
![image](https://github.com/Heterogeneity/Notes/assets/102458836/e7d54224-8cb6-421d-a9b6-3cd78b34630f)
2. 在文件目录栏中输入这个路径 C:\WINDOWS\System32，删除该路径下的以下三个文件：
  * java.exe
  * javaw.exe
  * javawa.exe
3. 删除`C:\ProgramData\Oracle`下的Java文件夹，删除之前安装JDK和JRE文件夹下的残留文件（没有就不用删）。
4. 进入`高级系统设置-环境变量`，删除CLASSPATH系统变量、JAVA_HOME系统变量和Path下的Java配置
![image](https://github.com/Heterogeneity/Notes/assets/102458836/837b29f7-ada4-4f51-81eb-d670e0c560ec)
![image](https://github.com/Heterogeneity/Notes/assets/102458836/0b0d69de-19a1-42f0-b52f-ea74c5bf5742)
![image](https://github.com/Heterogeneity/Notes/assets/102458836/5cd63b0d-fd46-4a28-b428-86f1da02a92b)
5. Win+r输入regedit打开注册表，删除HKEY_CURRENT_USER\Software\JavaSoft和HKEY_LOCAL_MACHINE\SOFTWARE\JavaSoft
6. 重启电脑，Win+r输入`cmd`打开命令行输入java -version，提示命令找不到即确认删除成功。

