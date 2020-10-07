# How to Use Quectel Modules

----------

## Quectel ##

Quectel Wireless Solutions is the leading global supplier of cellular and GNSS modules, with a broad product portfolio covering the most recent wireless technologies of 5G, LTE/LTE-A, NB-IoT/LTE-M, UMTS/HSPA(+), GSM/GPRS and GNSS. As a professional IoT (Internet of Things) technology developer and cellular module supplier, Quectel is able to provide one-stop services for IoT cellular modules. Quectel products have been widely applied in IoT/M2M fields including smart payment, telematics and transport, smart energy, smart cities, security, wireless gateways, industry, healthcare, agriculture, and environment monitoring.

作为全球领先的5G、LTE/LTE-A、NB-IoT/LTE-M、车载前装、安卓智能、GSM/GPRS、WCDMA/HSPA(+)和GNSS模组供应商，移远通信拥有的多样性的产品及其丰富功能充分满足了不同市场智能终端的需求。公司立足移动通信模组领域近十年，客户遍及各行各业，产品广泛应用于无线支付、车载运输、智慧能源、智慧城市、无线网关、工业应用、医疗健康和农业环境等众多领域，为全球市场物联网终端提供了通信模组解决方案。


![](https://www.quectel.com/images/about/ban2.png)

## Products ##

移远致力于为客户提供优质的产品和服务，满足客户各类需求如

- 功耗
- 速率
- 易用
- 兼容


## 技术篇 ##
通信模组的作用是借助SIM卡和移动网络，为嵌入式设备提供数据通信功能的设备。而从嵌入式本身角度出发，移远通信模组其实是以SoC芯片（Qualcom\Huawei\ASR\MTK...）为基础加上各种外围电路（PMU、RF电路、RAM/ROM等）运行可以实现数据通信的最小系统核心电路板。SoC上包括AP和DSP两类芯片，AP芯片运行Linux/Android/ThreadX等系统，DSP可以理解为modem或称BB（BaseBand）、BP（BaseBand Processor）。

由于AP上运行着嵌入式操作系统，并且有各类外设接口（Uart/SPI/I2C/Audio/SDIO等），因此移远模组也可以当作一个处理器使用，客户可以集成和移植外设和应用，用于一些对处理性能、资源要求不高的场景，移远称之为Open方案。

一般来说，移远模组是通过USB/PCIe/Uart连接另外一个嵌入式处理器，称之为Host AP，或EAP、External AP，移远通信模组作为一个设备使用，在Host AP上移植模组使用需要的驱动程序，这种方案称之为标准模组方案。

![Quectel_modules_stuff.png](https://i.loli.net/2020/09/29/bk4wJGADRZiYxst.png)

----------
Links:

*[`Official Website`](https://www.quectel.com/ "Official Website")*

*[`Forums`](https://Forums.quectel.com/ "Forums")*

