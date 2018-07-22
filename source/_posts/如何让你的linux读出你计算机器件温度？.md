title: '如何让你的linux读出你计算机器件温度？'
date: 2010-12-26 00:03:00
tags: LINUX
---


前段时间本想根据温度来调节功耗的，所以就想看看linux下给出的接口，能否方便的读取，进而调节。
刚开始在ubuntu下运行

```
#sensors
```

提示没有安装相关的驱动。
这时要运行sensors-detect

```
#sensors-detect
```

其中只要回答yes即可，最后的出现

```
Do you want to overwrite /etc/sysconfig/lm_sensors? (YES/no): no
To load everything that is needed, add this to one of the system
initialization scripts (e.g. /etc/rc.d/rc.local):

#----cut here----
# Chip drivers
modprobe coretemp
modprobe it87
/usr/local/bin/sensors -s
#----cut here----
```

如果马上想用不想重启，那么就直接使用modprobe安装相关的驱动。 即

```
#modprobe coretemp
#modprobe it87
```


最后来验证，

```
#sensors

coretemp-isa-0000
Adapter: ISA adapter
Core 0:      +30.0°C  (high = +76.0°C, crit = +100.0°C)

coretemp-isa-0001
Adapter: ISA adapter
Core 1:      +29.0°C  (high = +76.0°C, crit = +100.0°C)

it8718-isa-0a10
Adapter: ISA adapter
in0:         +1.07 V  (min =  +0.03 V, max =  +0.02 V)   ALARM
in1:         +2.16 V  (min =  +0.00 V, max =  +4.08 V)
in2:         +2.16 V  (min =  +0.00 V, max =  +4.08 V)
in3:         +3.02 V  (min =  +0.00 V, max =  +4.08 V)
in4:         +2.16 V  (min =  +0.00 V, max =  +4.08 V)
in5:         +0.06 V  (min =  +0.00 V, max =  +4.08 V)
in6:         +0.10 V  (min =  +0.00 V, max =  +4.08 V)
in7:         +2.96 V  (min =  +0.00 V, max =  +4.08 V)
Vbat:        +3.30 V
fan1:       1391 RPM  (min =   10 RPM)
fan2:       1220 RPM  (min =    0 RPM)
fan3:          0 RPM  (min =    0 RPM)
temp1:       +19.0°C  (low  = +127.0°C, high = +127.0°C)  sensor = thermal diode
temp2:       +28.0°C  (low  = +127.0°C, high = +127.0°C)  sensor = thermistor
temp3:       -70.0°C  (low  = +127.0°C, high = +127.0°C)  sensor = thermal diode
cpu0_vid:   +0.000 V

```

成功，呵呵。


当然，可以参考下面的文章，也有相关的介绍：

```
http://forums.linuxmint.com/viewtopic.php?f=42&t=31978&start=0#p184022
http://www.thinkwiki.org/wiki/Thermal_sensors
http://www.lm-sensors.nu/
```
		
		
		
		
		
		
		                                   