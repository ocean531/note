#### socket can是什么
socketcan吧CAN总线包装成了一种linux socket可以操作的网络接口







#### canframe
socketcan中基础的结构体
```cpp
#include<linux/can.h>

struct can_frame
```
frame.can_id , 就是CANID
frame.can_dlc，表示该can帧中有8个数据字节
frame.data[]，表示数据

#### 创建虚拟can
```bash
sudo modprobe vcan

sudo ip link add dev vcan0 type vcan
sudo ip link set up vcan0
```



#### 使用socat创建虚拟串口
```
socat -d -d pty,raw,echo=0 pty,raw,echo=0
```
使用这个指令，可以出