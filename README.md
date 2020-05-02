# Lenovo XiaoXinPro13 2109&2020 hackintosh


Lenovo XiaoXinPro13 Hackintosh

## 电脑配置
|规格 | 详细信息|
|:-: | :-:|
|电脑型号|联想小新pro13 笔记本电脑|
|操作系统|macOS Catalina 10.15.x |
|处理器|英特尔 酷睿 i7-10710U|
|内存|16GB板载无法更换|
|硬盘|原装 三星981A 512GB 更换为 三星970 evo 1TB |
|显卡|Intel HD Graphics CFL CRB|（UHD620）|
|显示器|13.3 英寸 IPS 2560x1600 华星光电|
|声卡| Realtek ALC257|
|网卡| 原装Intel AX201NGW更换为 BCM943602CS|

## 使用说明

### 注意

- 强烈建议不要使用`OpenCore Configurator`来修改`config.plist` `OpenCore Configurator`更新缓慢与`OpenCore`版本不匹配，推荐使用`ProperTree`   
- 小新由于安装过程中触摸板可能无法驱动，使用U盘安装macOS会占用仅仅一个USB接口,建议安装之前先买个usb拓展,用于插入鼠标,来进行安装步骤选项设定。
- 安装或更新系统完成后请使用`终端`输入`sudo kextcache -i /`清理缓存并重启，触控板才能正常使用

### BISO设置 

- BIOS 版本:  `CLCN32WW`
  - `Fn+F2`进入`BIOS`,
  - 先查看 `Information`：`Secure Boot` 是否为 `Disabled`;
  - 如果 `Secure Boot` 是 `Enabled`，选择左边到 `Security`： 设置 `Secure Boot` 为 `Disabled`;
  - `Fn+F10` 保存设置

### 配置config【重要】

- 已修改BIOS`DVMT`机器可删除以下内容
  - `Kernel` \ `Patch` \ `item0`
  - `Kernel` \ `Patch` \ `item1`
  - `PciRoot(0x0)/Pci(0x2,0x0)`\ `framebuffer-fbmem` = `00009000`
  - `PciRoot(0x0)/Pci(0x2,0x0)`\ `framebuffer-stolenmem` = `00003001`
  
- 已解锁BIOS`CFG LOCK`机器将以下内容`False` 即可
  - `Kernel` \ `Quirks` \ `AppleXcpmCfgLock`=`False`
  - `Kernel` \ `Quirks` \ `AppleXcpmExtraMsrs`=`False` 
    
- 使用`1820A`网卡的驱动蓝牙和WI-FI将这些`True`即可 并添加 `brcmfx-country=#a`
  - `Kernel` \ `Add` \ `18`\ `Enabled`=`True`
  - `Kernel` \ `Add` \ `19`\ `Enabled`=`True`
  - `Kernel` \ `Add` \ `20`\ `Enabled`=`True`
  - `Kernel` \ `Add` \ `21`\ `Enabled`=`True`
  - `NVRAM` \ `Add` \ `7C436110-AB2A-4BBB-A880-FE41995C9F82` \ `boot-args` \ `brcmfx-country=#a`
 
- 部分`i5`机型可删除以下内容
  - `Kernel` \ `Patch` \ `item0`
  - `Kernel` \ `Patch` \ `item1`
  - `Kernel`\ `Emulate`\ `Cpuid1Data`
  - `Kernel`\ `Emulate`\ `Cpuid1Mask`  

### 安装失败注意

- 强烈建议解锁BIOS`CFG LOCK` `DVMT`以避免安装时卡住。解锁方法请参考群文件`小新PRO13修改DVMT说明` 

- 安装系统时请在BIOS中禁用无线网卡，安装成功后在打开。避免因网卡问题导致安装失败
  - 一些网卡需要屏蔽针脚，方法请自行百度
    
- 打开`CPU多线程(BIOS Hyper)`时可能导致`-v`引导失败，可尝试 （@宪武）
  - `UEFI`-`Quirks`-`ReleaseUsbOwnership`=`True`
   
- 安装`macOS Catalina10.15.4`过程中可能无法驱动核显，导致引导失败重启回引导页面，临时解决方法
  
  - `DeviceProperties` \ `Add` \ `PciRoot(0x0)/Pci(0x2,0x0)`\ `AAPL,ig-platform-id`=`12345678` 安装成功后恢复即可

### 关闭触摸板快捷键

- 组合键: FN+F6

### 唤醒方法

- 电源键

### 不正常工作

- ~~睡眠~~ (小新PRO13不能真正睡眠，可以仿真睡眠。唤醒比较困难，`OC` 下唤醒方法是：`电源键`唤醒)
- 声卡MIC(`暂时解决方法`：启动台-声音-输入：手动切换到“`外接可用麦克风设备`”)
<details>
<summary>关于 小新PRO13(2019/2020/13S Intel版本) 没有S3睡眠延展</summary>
<p>D0 就是正常工作状态，S0 是 D0 的电源管理，S0睡眠应该是不存在的，说 S0 睡眠，本质就是 D0 状态下进入了空闲，所以有了空闲状态下的电源管理，这个机器没有 S3睡眠，没有设计相关硬件</p>
<p>但因 ACPI 有了 S3才导致苹果试图进入睡眠，但因缺少必须的硬件最终失败，对于 Windows 不妨碍</p>更详细的说明移步<a href="https://github.com/daliansky/OC-little/tree/master/01-%E5%85%B3%E4%BA%8EAOAC" target="_blank">OC-little</a>
<p>实测选择质量好的SSD或无线网卡可有效延长待机时间。如：三星970EVO+DW1820A盒盖一小时耗电仅需0.88%</p>   
</details>

### 哪些可以工作更好
- 开启 [HIDPI](https://github.com/xzhih/one-key-hidpi) 来提升系统UI质量, `可能会出现花(黑)屏现象`

### 镜像下载
  
- [[**黑果小兵的部落阁**] :【黑果小兵】原版镜像](https://blog.daliansky.net/categories/下载/镜像/)

### EFI下载

- [OpenCore](https://github.com/Hush-vv/Lenovo-XiaoXinPro13-Hackintosh/archive/master.zip)
   
- Clover[请移驾daliansky](https://github.com/daliansky/XiaoXinPro-13-hackintosh)
        
### 感谢
- 本EFI所使用的`ACPI`均来自 @宪武 大佬
- daliansky黑果小兵
- 感谢PS@Donald提供的解锁`DVMT` `CFG lock`工具
- 感谢群友QQ876310253提供的`解锁dvmt及cfglock.docx`教程    
- 感谢群友Dreamn提供的[`SleepWithoutBluetoothAndWifi`](https://github.com/dreamncn/SleepWithoutBluetoothAndWifi)工具        
- @Bat.bat [自动化 OpenCore 编译，每 8 小时刷新一次](https://github.com/williambj1/OpenCore-Factory/releases)
    
    ......

### QQ群
- 小新pro黑苹果 946132482（已满）
    
- 小新pro13insyde bios研究交流 635160015
        

### 更新日志  
  
- [Changelog](https://github.com/Hush-vv/Lenovo-XiaoXinPro13-Hackintosh/blob/master/%E6%9B%B4%E6%96%B0%E6%97%A5%E5%BF%97.md)
