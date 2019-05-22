厂测校准可以请客户参考文档：
详细：00014010-WS-170731-RTL8723D_COB_MP_FLOW_R04.pdf  //主要针对COB
概况：Quick_Start_Guide_V6.txt						   //主要针对模组

参考RTL8723DS_RF测试，这里说下如果是其他模组，需要改动点：
把mp_rtl8723d_config/mp_rtl8723d_fw文件拷贝到external\rkwifibt\realtek\RTL8723DS
假设你使用的RTL8821CS模组，则需要变为：
把mp_rtl8821cs_config/mp_rtl8821cs_fw文件拷贝到external\rkwifibt\realtek\RTL8821CS //mp*文件找模组厂要

注意：
1、如果SDK默认没有支持你们使用的模组，请参考《Realtek移植到RK平台移植说明.txt》进行移植，确保可以正常使用；然后再进行后续操作
2、测试文档里面用的工具都已集成，且wifi驱动都是buildin的，所以文档里面的前期步骤都可以省略（比如remount、root、insmod等等之类都可以省略），直接进入测试章节即可；
3、文档iwpriv之类都必须换成rtwpriv，如果遇到某些命令执行不了或软件相关问人体请先咨询软件工程师，最好软硬件协同测试。
4、如果你们没有专业的wifibt测试仪器，可以请你们找拿货的wifi代理商，请代理商教你们或者到代理处去做RF测试。
5、做完RF测试，可以把均值map写进efuse map，可以选择不把efuse map写进wifi ic，而用读档的方式，即把efuse map放在外部一个固定的存储路径（需保证系统升级不被覆盖掉）;
   读档方法参考REALTEK_COB_加载map文件.txt