AP模组RF测试：

1、首先要确保SDK有支持你们选择模组，如果不支持请参考：《AP模组移植到RK平台移植说明.txt》进行移植，然后buildroot配置选择对应的wifi

2、只有在第一步有正确配置的前提下， 才会自动生成蓝牙初始化文件：bt_pcba_test文件（参考buildroot/package/rockchip/rkwifibt/rkwifibt.mk）

其他参考AP6236 RF测试, 需要修改的地方如下：

1、测试firmware：每一个ap模组对应的测试firmware都是不一样的，比如：
	AP6236  -> fw_bcm43436b0_mfg.bin
	AP6212A -> fw_bcm43438a1_mfg.bin
	所以这个要改下：fw_bcmxxx_mfg.bin 和 APxxxx 都要根据你的模组型号进行相应调整
	“把这个文件替换到fw_bcmxxx_mfg.bin 放到到 external/rkwifibt/firmware/broadcom/APxxxx/wifi/这个目录”

2、蓝牙的测试脚本bt_pcba_test的变动，测试前请检查下内容是否正确：（文件路径: /usr/bin/bt_pcba_test），内容如下：
	brcm_patchram_plus1 ... ... /system/etc/firmware/BCM4343B0.hcd /dev/ttyS4 &
	注意检查：BCM4343B0.hcd要跟具体的模组蓝牙型号对应（可以找模组厂确认），ttySX其实就是：uartX硬件上蓝牙连接主控哪一个uart口。