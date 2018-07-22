title: 'IBM i5-os ip 配置'
date: 2011-12-12 18:41:27
tags: 系统运维
---

转自：http://www.ifeeling.net/article.asp?id=565

```
一、配置网卡的IP地址
1、查看网卡的资源名称
输入DSPHDWRSC *CMN ，显示通信资源信息：
引用内容 引用内容

                       Display Communication Resources                         
                                                             System:    
Type options, press Enter.                                                     
   5=Display configuration descriptions   7=Display resource detail             
                                                                                
Opt  Resource        Type  Status                Text                          
      CMB01           286D  Operational           Combined function IOP         
        LIN01         2771  Operational           Comm Adapter                  
          CMN01       2771  Operational           Comm Port                     
          CMN02       2771  Operational           V.24 Port                     
        LIN02         2838  Operational           LAN Adapter                   
          CMN03       2838  Operational           Ethernet Port                 
                                                                     
                                                                         Bottom 
F3=Exit   F5=Refresh   F6=Print   F12=Cancel                                   

根据Text的描述，找到Ethernet Port的资源名就是网卡。系统可能会有几个Ethernet port，此处为CMN03。

2、为该网卡资源创建线描述
输入CRTLINETH，在出现的提示框中输入：
Line Description:TCPIP
Resource name: CMN03
回车，继续设置速度与双工，一般都设为*AUTO，回车后，出现“ Line description TCPIP created.”  提示，说明创建成功了。


3、查看线描述，并VARY ON 线描述
输入WRKLIND ,在TCPIP前输入8 (=Work with status)，查看当前状态为VARIED OFF.
输入1(=Vary on)并回车，显示为VARY ON PENDING
F12退回，再次查看，已经是VARY ON的状态了。

4、配置IP地址
输入ADDTCPIFC，在界面中输入IP地址，掩码和线描述TCPIP，回车后提示： TCP/IP interface added successfully. 


5、查看并激活IP地址
CFGTCP回车，选择 1. Work with TCP/IP interfaces，可以看到刚才设置好的IP地址。
按 F11=Display interface status，可以看到当前IP的状态是INACTIVE
选9=Start，提示： Activating TCPIP to start IP 192.168.33.3 for QSECOFR in 000979/QSECOFR/D...

F12回退，再显示IP状态，已经是Active。

此时TCPIP已经创建，IP地址已经激活。找个同网段的主机Ping一下，测试成功。


二、设置AS400的路由
1、设置默认路由
输入CFGTCP 再选择  2. Work with TCP/IP routes 
在   Work with TCP/IP Routes 界面中，输入1=ADD，添加一条默认路由。
注意默认路由的写法。
Route destination:输入 *DFTROUTE  注意不能带单引号
Subnet mask：输入 *NONE 注意不能带单引号
Next Hop：输入网关的IP地址
确认回车后可以看到新添加的默认路由。


2、添加其它路由
同上，直接输入相应的设置，输入时无需单引号。
```