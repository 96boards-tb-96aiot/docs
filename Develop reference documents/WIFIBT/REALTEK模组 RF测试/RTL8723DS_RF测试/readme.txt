1、修改rkwifibt.mk， 注意最新2019年5月份SDK已经集成该补丁，所以最新SDK不需要打
buildroot/package/rockchip/rkwifibt$ git diff
diff --git a/package/rockchip/rkwifibt/rkwifibt.mk b/package/rockchip/rkwifibt/rkwifibt.mk
index 1d715de..3c280aa 100644
--- a/package/rockchip/rkwifibt/rkwifibt.mk
+++ b/package/rockchip/rkwifibt/rkwifibt.mk
@@ -108,6 +108,7 @@ define RKWIFIBT_ENABLE_BT
     $(INSTALL) -D -m 0755 $(RKWIFIBT_BIN_DIR)/rtlbtmp $(TARGET_DIR)/usr/bin/rtlbtmp
 
     mkdir -p $(TARGET_DIR)/lib/firmware/rtlbt/
+    $(INSTALL) -D -m 0644 $(@D)/realtek/$(CHIP_NAME)/mp* $(TARGET_DIR)/lib/firmware/
     $(INSTALL) -D -m 0644 $(@D)/realtek/$(CHIP_NAME)/* $(TARGET_DIR)/lib/firmware/rtlbt/
     sed -i 's/BT_TTY_DEV/\/dev\/$(BT_TTY_DEV)/g' $(@D)/bt_load_rtk_firmware
     $(INSTALL) -D -m 0755 $(@D)/bt_load_rtk_firmware $(TARGET_DIR)/usr/bin/bt_pcba_test
@@ -116,6 +117,7 @@ define RKWIFIBT_ENABLE_BT
     $(INSTALL) -D -m 0755 $(@D)/bt_realtek_hfp_hanup $(TARGET_DIR)/usr/bin/bt_realtek_hfp_hanup
     $(INSTALL) -D -m 0755 $(@D)/bt_realtek_hfp_accept $(TARGET_DIR)/usr/bin/bt_realtek_hfp_accept
     $(INSTALL) -D -m 0644 $(@D)/realtek/bluetooth_uart_driver/hci_uart.ko $(TARGET_DIR)/usr/lib/modules/hci_uart.ko
+    rm -rf $(TARGET_DIR)/usr/bin/bt_load_rtk_firmware
     ln -s $(TARGET_DIR)/usr/bin/bt_pcba_test $(TARGET_DIR)/usr/bin/bt_load_rtk_firmware
 endef
 
2、把mp_rtl8723d_config/mp_rtl8723d_fw文件拷贝到external\rkwifibt\realtek\RTL8723DS

3、make rkwifibt-rebuild 重新打包固件

4、开机后确保ls lib/firmware/ 确保这个目录下存在mp_rtl8723d_config/mp_rtl8723d_fw两个文件

5、
5.1、WiFi测试校准
参考：RK3308 WiFi射频测试简述.docx 、WS-170731-RTL8723D_COB_MP_FLOW_R03.pdf 

5.2、BT测试请参考MP tool user guide for linux20180319.pdf（里面的具体测试内容请咨询模组厂或者原厂）
注：执行测试前请注意：
先打开bt电源：
echo 0 > sys/class/rfkill/rfkill0/state
sleep 1
echo 1 > sys/class/rfkill/rfkill0/state
另外rtk_hciattach这个进程不能跑，如果有请杀掉killall rtk_hciattach

///////////
/ #
/ # rtlbtmp
:::::::::::::::::::::::::::::::::::::::::::::::::
:::::::: Bluetooth MP Test Tool Starting ::::::::
>
>
> enable uart:/dev/ttyS4   //注意ttySX 对应 uart口
> > > enable[Success:0]

