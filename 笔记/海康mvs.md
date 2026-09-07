![[Pasted image 20260907181421.png]]

#### 初始化SDK
在进程中调用SDK，首先要进行初始化SDK的运行环境，以提升后续接口调用的流程性。
```
int main(){
	int nRet = MV_OK; //初始化SDK
	MV_CC_Initialize();
	
	//
	进行设备发现、控制，图像采集等操作；
	//
	
	MV_CC_Finalize(); //程序退出时，反初始化SDK，释放SDK所占的资源
	}
```
在单个进程中，仅可执行该套流程一次

#### 初始化相机
##### 1.枚举相机
```
MV_CC_DEVICE_INFO_LIST stDeviceList ; //这是一个结构体，包含在线设备数量和在线设备两个成员

//枚举GigE和USB相机
memset(&stDeviceList,0,sizeof(MV_CC_DEVICE_INFO_LIST));
nRet = MV_CC_EnumDevices(MV_GIGE_DEVICE|MV_USB_DEVICE,&stDeviceList); //需要传入对应的设备接口类型nTLayerType

Check(nRet);

//还有两种枚举方式，可以过滤和进行排序
```
[[c++#memset（）]]
部分可选设备接口类型以及对应的枚举设备如下：
![[Pasted image 20260907183427.png]]

###### MV_CC_DEVICE_INFO_LIST 结构体

##### 2.创建相机实例
创建相机实例，调用MV_CC_CreateHandle(),需传入设备信息pstDevinfo。
```
void* handle = NULL;

//选择设备并创建句柄
nRet = MV_CC_CreateHandle(&handle,stDeviceList.pDeviceInfo[nIndex]);

Check(nRet);
```

##### 3.打开相机
建立相机实例与物理相机的连接，使相机实例获得访问物理相机的权限，实现通信
```
//打开相机
MV_CC_OpenDevice(handle);

Check(nRet);
```
该方式默认相机实例独占物理相机的访问权限
##### 设置参数（可选）
##### 4.关闭相机
调用MV_CC_CloseDevice(handle)
```
nRet = MV_CC_CloseDevice(handle);
Check(nRet);
```
##### 5.销毁相机实例
```
nRet = MV_CC_DestroyHandle(handle);
Check(nRet);

```