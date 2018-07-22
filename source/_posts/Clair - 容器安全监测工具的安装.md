title: Clair - 容器安全监测工具的安装
date: 2016-12-29 08:45:21
tags: 网络与安全
---


	这里为了方便网络的配置，使用 host net 方案，

docker run  --net host --name postgresql -itd --restart always   --env 'PG_PASSWORD=yourpassword'   sameersbn/postgresql:9.5-3

2. 创建 clair 的配置

curl -L https://raw.githubusercontent.com/coreos/clair/v1.2.6/config.example.yaml -o $HOME/clair_config/config.yaml

修改其中的 postgresql 配置， 类似如下：

source: postgresql://postgres:yourpassword@ip:5432?sslmode=disable

3. 运行 clair

docker run  -d -p 6060-6061:6060-6061 -v /root/clair_config/:/config  -v /tmp:/tmp  clair:dev  -config=/config/config.yaml

4. 这样clair 就运行了，clair 刚开始运行需要一段较长的时间来更新安全数据数据库，所以确保外网网络连接正常

5. 运行 clair 扫描工具
	
		
./analyze-local-images  a1553cdf672f

这里的 analyze 通过如下安装 

运行结果类似如下：


CVE-2016-0494 (High)
    Unspecified vulnerability in the Java SE and Java SE Embedded components in
    Oracle Java SE 6u105, 7u91, and 8u66 and Java SE Embedded 8u65 allows remote
    attackers to affect confidentiality, integrity, and availability via unknown
    vectors related to 2D.


    Package:       icu @ 52.1-8+deb8u3
    Fixed version: 52.1-8+deb8u4
    Link:          https://security-tracker.debian.org/tracker/CVE-2016-0494
    Layer:         8760f48ef252d37743b66f97f5d55646656a84a9e967d3a8028e09b3b7544106


CVE-2016-7167 (High)
    Multiple integer overflows in the (1) curl_escape, (2) curl_easy_escape, (3)
    curl_unescape, and (4) curl_easy_unescape functions in libcurl before 7.50.3
    allow attackers to have unspecified impact via a string of length 0xffffffff,
    which triggers a heap-based buffer overflow.


    Package:       curl @ 7.38.0-4+deb8u5
    Link:          https://security-tracker.debian.org/tracker/CVE-2016-7167
    Layer:         8760f48ef252d37743b66f97f5d55646656a84a9e967d3a8028e09b3b7544106
		
			
		
......


	

		
./analyze-local-images  a1553cdf672f

这里的 analyze 通过如下安装 

运行结果类似如下：


CVE-2016-0494 (High)
    Unspecified vulnerability in the Java SE and Java SE Embedded components in
    Oracle Java SE 6u105, 7u91, and 8u66 and Java SE Embedded 8u65 allows remote
    attackers to affect confidentiality, integrity, and availability via unknown
    vectors related to 2D.


    Package:       icu @ 52.1-8+deb8u3
    Fixed version: 52.1-8+deb8u4
    Link:          https://security-tracker.debian.org/tracker/CVE-2016-0494
    Layer:         8760f48ef252d37743b66f97f5d55646656a84a9e967d3a8028e09b3b7544106


CVE-2016-7167 (High)
    Multiple integer overflows in the (1) curl_escape, (2) curl_easy_escape, (3)
    curl_unescape, and (4) curl_easy_unescape functions in libcurl before 7.50.3
    allow attackers to have unspecified impact via a string of length 0xffffffff,
    which triggers a heap-based buffer overflow.


    Package:       curl @ 7.38.0-4+deb8u5
    Link:          https://security-tracker.debian.org/tracker/CVE-2016-7167
    Layer:         8760f48ef252d37743b66f97f5d55646656a84a9e967d3a8028e09b3b7544106
		
			
		
......


	
			
		