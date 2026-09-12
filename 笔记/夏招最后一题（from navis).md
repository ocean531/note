
在写这个题前，不如先学习一下同济的开源里面的一部分通信代码是怎么写的，再结合考题里面给到的知乎学习链接，我们就可以完成这个任务。

# 同济串口代码

在之前分析同济自瞄数据流里面的代码，main里面其实并没有直接对串口的设置和利用，只有 `gimbal.send` 这一段代码。这就不得不说到同济代码里面的封装之好，代码非常的整洁和没有什么冗杂。

```CPP
Gimbal::Gimbal(const std::string & config_path)

{
  auto yaml = tools::load(config_path);
  auto com_port = tools::read<std::string>(yaml, "com_port");

  try {
    serial_.setPort(com_port);
    serial_.setBaudrate(921600);
    serial_.open();
  } catch (const std::exception & e) {
    tools::logger()->error("[Gimbal] Failed to open serial: {}", e.what());
    exit(1);
  }

  thread_ = std::thread(&Gimbal::read_thread, this);
  tools::logger()->info("[Gimbal] First q received.");
}
```

这是Gimbal类的析构函数，通过这个析构函数，在创建Gimbal的时候就可以完成对串口的一些初始化，比如上面这一段就完成了：
- 设置串口名称 `setPort` 是 `com_port` ，在参数文件里面去设置对应的串口名称。
- 设置串口波特率 `setBaudrate` 为921600
- 尝试打开串口 `open`
- 当捕捉到异常的时候，就在终端上显示打开失败
至于serial的实例化，是在gimbal的hpp文件里面去做的 

```cpp
struct __attribute__((packed)) GimbalToVision
{
  uint8_t head[2] = {'S', 'P'};
  uint8_t mode;  // 0: 空闲, 1: 自瞄, 2: 小符, 3: 大符
  double bullet_speed;
  uint16_t crc16;
};

//···

  GimbalToVision rx_data_;
```

定义了底盘到小电脑的通信数据结构，而且用rx_data_去承接

```cpp
struct __attribute__((packed)) VisionToGimbal
{
  uint8_t head[2] = {'S', 'P'};
  uint8_t mode;  // 0: 不控制, 1: 控制云台但不开火，2: 控制云台且开火
  //uint8_t fire;
  double yaw;
  double pitch;
  uint16_t crc16;
};

//···

  VisionToGimbal tx_data_;
```

定义了小电脑到云台的通信数据结构，而且用tx_data_去承接


```cpp
#include <serial/serial.h>
#include <iostream>

struct data_package
{
    char start = 's';
    char unused1[2];
    float speed = 20;
    float euler[3] = {}; //(0,1,2) = (yaw,roll,pitch)
    char shoot_bool = 0;
    char RuneFlag = 0; //
    char unused2[10] = {};
    char end = 'e';
} __attribute__((packed));
static_assert(sizeof(data_package) == 32);

data_package data;
int main()
{
    std::cout << "helloworld" << std::endl;
    serial::Serial ser; // 实例化一个串口的对象
    ser.setPort("/dev/serial_sdk"); // 设置串口设备
    ser.setBaudrate(115200);        // 设置波特率
    try
    {
        ser.open(); // 打开串口
        while (true)
        {
            std::cout << "number" << ser.available() << std::endl; // 读取到缓存区数据的字节数
            ser.read(reinterpret_cast<uint8_t *>(&data), 32);//将data_package类型结构体强制转换位uint8_t类型的指针，来接收32字节的数据
            std::cout << data.start << data.unused1[0] << data.unused1[1] << std::endl;
            std::cout << "(yaw,pitch,roll)" << data.euler[0] << " " << data.euler[1] << " " << data.euler[2] << std::endl;
        }
    }
    catch (std::exception &e)
    {
        std::cerr << e.what() << std::endl;
    }
}
```

根据知乎的学习链接，我们完成一个串口通信的需求，至少需要做到以下步骤：
- 定义通信数据的内容，具体格式需要有起始位，数据位，停止位
- 串口的实例化，初始化设置的串口名称，波特率
- 尝试打开串口，传输数据

一些其他的知识点：
- 用 `write` 来发送数据，用 `read` 来接受数据，在serial的文件里面，分别有四个不同的函数重载，最终目的都是实现发送和接收数据，所以我们使用具体的一个就好了

根据具体的gimbal里面的代码，使用的是都是原始的指针传入的函数

```CPP
//WRITE
size_t Serial::write(const uint8_t * data, size_t size)
{
  ScopedWriteLock lock(this->pimpl_);
  return this->write_(data, size);
}

size_t Serial::write_(const uint8_t * data, size_t length) {
 return pimpl_->write(data, length); 
 }

//READ
size_t Serial::read(uint8_t * buffer, size_t size)
{
  ScopedReadLock lock(this->pimpl_);
  return this->pimpl_->read(buffer, size);
}
```

- 还可以使用 `available` 防止read因为长时间等待


根据这些内容，我们开始尝试写一个串口，我们尝试同时设置一下发送和接受格式，尝试接受和发送，学习情况先不做CRC校验

```CPP
#include "serial/serial.h"
#include <iostream>

struct Sendtest{
    uint8_t head[3]={'v','g','d'};
    float yaw;
    uint8_t control;
    uint8_t fire;

}__attribute__((packed));

struct Rectest{
    uint8_t head[3]={'v','g','d'};
    uint8_t mode;
    double bullet_speed;
    
}__attribute__((packed));


int main(){
    serial::Serial ser;
    ser.setPort("/dev/ttyUSB0");
    ser.setBaudrate(115200);
    ser.open();

    Sendtest tx;
    tx.yaw=1.0;
    tx.control=1;
    tx.fire=1;

    Rectest rx;

    while (1)
    {
        ser.write((const uint8_t*)&tx,sizeof(tx));
        if (ser.available() >= sizeof(Rectest))
        {
        ser.read((uint8_t*)&rx,sizeof(rx));
        }
    }
    
}

```




