title: 'SAS 和 SATA 比较'
date: 2013-03-10 11:34:35
tags: 云计算
---

SAS和SATA有相似的技术点，都是对传统的并行接口进行了改进，SCSI, ATA

```

应用领域                    容量                   性能              可靠性                           价格 

SAS     服务器 （企业级）   最高都达到 4TB          6GB/s         1.4 million hour MTBF                高
                                                可持续顺序
                                               数据速率 182 MB/s
                                                                                            


SATA   桌面，工作站          同上             3GB/s， 6GB/s        1.2 million hour MTBF              低
                                                可持续顺序
                                             数据速率 171 MB/s


SAS drive cables can extend up to six times the length of SATA drive cables, and SAS drives are dual ported while SATA drives can only 
communicate via one port
```

参考资料:

1. http://blog.lewan.com/2009/09/14/sas-vs-sata-differences-technology-and-cost/
2. http://download.intel.com/design/iio/docs/31512701.pdf
3. http://www.wdc.com/en/products/products.aspx?id=580
4. http://www.wdc.com/en/products/products.aspx?id=30