# 背景

该问题在完成大数据实验课任务中出现，要使用vmware搭建hadoop平台。搭建Hadoop平台具体教程参见视频[黑马程序员Hadoop教程](https://www.bilibili.com/video/BV1WY4y197g7/?spm_id_from=333.337.search-card.all.click&vd_source=bd9b0c20658086a80ddce73476f5c881)

# 问题描述与解决过程

使用VMware运行centos系统发现，虚拟机启动后搁几分钟就卡死，起初以为是内存原因，遂把原虚拟机内存由1G调高至4G，问题仍然存在。

上网查阅，根据几篇博客如 (https://blog.csdn.net/weixin_62952541/article/details/130229182) 操作，仍不能解决问题。

决定重装VMware（借助CCleaner清理注册表，手动删了不少东西才差不多删干净），当天晚上效果良好，到了实验课给老师演示的时候直接又卡死了

最后通过[这篇博客](https://blog.csdn.net/ss810540895/article/details/134189242?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-1-134189242-blog-135236536.235^v43^pc_blog_bottom_relevance_base3&spm=1001.2101.3001.4242.2&utm_relevant_index=4)
解决了该问题。

# 具体解决步骤

1. 关闭虚拟机
2. 打开`编辑虚拟机设置`选项
3. 选择硬件-处理器，勾选`虚拟化inteltv-x/ept或amd-v/rvi`
4. 启动虚拟机，问题解决

如果启动虚拟机提示`此平台不支持虚拟化的Intel VT-x/EPT. 不使用虚拟化的Intel VT-x/EPT,是否继续？`，选择否，继续如下步骤：

1. 打开任务管理器，选择性能-cpu，查看下方是否有`虚拟化：已启用`字样，如果有，继续下一步，否则关闭电脑，打开BIOS，在BIOS界面开启VT功能，开启电脑
2. Win+q，在搜索栏中搜索`启用或关闭Windows功能`，打开界面后，关闭如下几项：
    * Hyper-V
    * Windows沙盒
    * 虚拟机平台
    * 适用于Linux的Windows子系统
总之要确保Windows本地的虚拟化功能完全关闭
3. Win+q，搜索`内核隔离`，确保`DMA`选项关闭(或打开Windows安全中心-设备安全性-内核隔离)
4. Win+r，输入`cmd`打开命令行，执行`bcdedit /set hypervisorlaunchtype off`关闭所有Hyper-V服务(Hyper-V与VMware实际上不兼容)
5. 重启计算机，打开VMware，转到`打开编辑虚拟机设置选项`。

如果仍然提示`此平台不支持虚拟化的Intel VT-x/EPT. 不使用虚拟化的Intel VT-x/EPT,是否继续？`，百度搜索如何关闭Windows的Hyper-V，一定确保所有服务和功能关闭。
