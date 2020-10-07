# ttyUSB序号的问题解决方法 #

遇到一些问题，导致ttyUSB0~ttyUSB3 并非对应DM、GPS、AT、PPP的情况，包括

	接入了另外一个模组（VID/PID不同），无法区分
	接了USB串口，占用了ttyUSB0
	应用程序出错了。打开ttyUSBx出错，但未关闭ttyUSBx，设备重新枚举时，ttyUSBx资源未释放
	
## 重置USB总线 ##

参考 

[解决自动化测试设备掉线：软件方案](https://testerhome.com/topics/9172)

[How to Reset USB Device in Linux](https://blog.csdn.net/mirkerson/article/details/9047831)

<details>
<summary>reset.c</summary>
<pre><code>	

	/*重启usb硬件端口*/
	#include <stdio.h>
	#include <unistd.h>
	#include <fcntl.h>
	#include <errno.h>
	#include <sys/ioctl.h>
	#include <linux/usbdevice_fs.h>

	int main(int argc, char **argv)
	{
	    const char *filename;
	    int fd;
	    int rc;
	
	    if (argc != 2) {
	        fprintf(stderr, "Usage: usbreset device-filename\n");
	        return 1;
	    }
	    filename = argv[1];//表示usb的ID
	
	    fd = open(filename, O_WRONLY);
	    if (fd < 0) {
	        perror("Error opening output file");
	        return 1;
	    }
	
	    printf("Resetting USB device %s\n", filename);
	    rc = ioctl(fd, USBDEVFS_RESET, 0);//ioctl是设备驱动中，对I/O设备进行管理的函数
	    if (rc < 0) {
	        perror("Error in ioctl");
	        return 1;
	    }
	    printf("Reset successful\n");
	
	    close(fd);
	    return 0;
	}

</code></pre>
</details>

gcc -o reset reset.c
sudo ./reset /dev/bus/usb/006/002

可以看到设备重新枚举

## udev规则固定ttyUSB ##

可以通过添加udev规则的方式。譬如，接上EC25模组，希望驱动加载后，生成的端口固定。

创建文件 /etc/udev/99-quectel-EC25.rules 内容如下
<details>
<summary>99-quectel-EC25.rules</summary>
<pre><code>
	SUBSYSTEMS=="usb", ENV{.LOCAL_ifNum}="$attr{bInterfaceNumber}"
	
	SUBSYSTEMS=="usb", KERNEL=="ttyUSB[0-9]*", ATTRS{idVendor}=="2c7c", ATTRS{idProduct}=="0121", ENV{.LOCAL_ifNum}=="02", SYMLINK+="EC21.AT", MODE="0660"
	SUBSYSTEMS=="usb", KERNEL=="ttyUSB[0-9]*", ATTRS{idVendor}=="2c7c", ATTRS{idProduct}=="0121", ENV{.LOCAL_ifNum}=="03", SYMLINK+="EC21.MODEM", MODE="0660"
	
	SUBSYSTEMS=="usb", KERNEL=="ttyUSB[0-9]*", ATTRS{idVendor}=="2c7c", ATTRS{idProduct}=="0125", ENV{.LOCAL_ifNum}=="01", SYMLINK+="EC25.NMEA", MODE="0660"
	SUBSYSTEMS=="usb", KERNEL=="ttyUSB[0-9]*", ATTRS{idVendor}=="2c7c", ATTRS{idProduct}=="0125", ENV{.LOCAL_ifNum}=="02", SYMLINK+="EC25.AT", MODE="0660"
	SUBSYSTEMS=="usb", KERNEL=="ttyUSB[0-9]*", ATTRS{idVendor}=="2c7c", ATTRS{idProduct}=="0125", ENV{.LOCAL_ifNum}=="03", SYMLINK+="EC25.MODEM", MODE="0660"

</code></pre>
</details>
这样插入EC25模组后，会自动生成链接，即使ttyUSB*已经出错了。用户应用程序可以适用/dev/EC25.AT、/dev/EC25.MODEM、/dev/EC25.NMEA

![udevEC25.png](https://i.loli.net/2020/09/30/F2sS8AgOlG4XZaD.png)



----------
<button onclick="window.location.href = '../Applications.html'"><font color=0x00FF >返回上页</font></button>









