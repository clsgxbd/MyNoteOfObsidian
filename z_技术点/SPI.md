# SPI
#SPI

##  #SPI 
- SPI是串行外设接口(Serial Peripheral Interface) 的缩写. SPI, 是一种高速的, 全双工, 同步的通信总线, 并且在芯片的管脚上只占用四根线, 节约了芯片的管脚, 同时为PCB的布局上节省空间, 提供方便, 正是出于这种简单易用的特性, 如今越来越多的芯片集成了这种通信协议, 比如AT91RM9200.

##   #SPI协议 
- SPI协的通信原理很简单, 它以主从方式工作, 这种模式通常有一个主设备和一个或多个从设备, 需要至少4根线, 事实上3根也可以(单向传输时). 也是所有基于SPI的设备共有的, 他们是SDI(数据输入) , SDO(数据输出) , SCLK(时钟), CS(片选) .
	- (1) SDI - SerialData In, 串行数据输入;
	- (2) SDO - SerialData Out, 串行数据输出;
	- (3) SCLK - Serial Clock, 时钟信号, 由主设备产生;
	- (4) CS - Chip Select, 从蛇别使能吸纳后, 由主设备控制;
- 其中, CS 是从芯片是否被主芯片选中的控信号, 也就是说只有片选信号为预先规定的使能信号时(高电位或低电位), 主芯片对此从新拍呢的操作才有效. 这就使在同一条总线上连接多个SPI设备成为可能.
- 接下来就是负责通讯的3根线了.通讯是通过数据交换完成的，这里先要知道SPI是串行通讯协议，也就是说数据是一位一位的传输的。这就是SCLK时钟线存在的原因，由SCLK提供时钟脉冲，SDI，SDO则基于此脉冲完成数据传输。数据输出通过 SDO线，数据在时钟上升沿或下降沿时改变，在紧接着的下降沿或上升沿被读取。完成一位数据传输，输入也使用同样原理。因此，至少需要8次时钟信号的改变（上沿和下沿为一次），才能完成8位数据的传输。
- SCLK信号线只由主设备控制，从设备不能控制信号线。同样，在一个基于SPI的设备中，至少有一个主控设备。这样传输的特点：这样的传输方式有一个优点，与普通的串行通讯不同，普通的串行通讯一次连续传送至少8位数据，而SPI允许数据一位一位的传送，甚至允许暂停，因为SCLK时钟线由主控设备控制，当没有时钟跳变时，从设备不采集或传送数据。也就是说，主设备通过对SCLK时钟线的控制可以完成对通讯的控制。SPI还是一个数据交换协议：因为SPI的数据输入和输出线独立，所以允许同时完成数据的输入和输出。不同的SPI设备的实现方式不尽相同，主要是数据改变和采集的时间不同，在时钟信号上沿或下沿采集有不同定义，具体请参考相关器件的文档。
- 最后，SPI接口的一个缺点：没有指定的流控制，没有应答机制确认是否接收到数据。
- SPI的片选可以扩充选择16个外设,这时PCS输出=NPCS,说NPCS0~3接4-16译码器,这个译码器是需要外接4-16译码器，译码器的输入为NPCS0~3，输出用于16个外设的选择。

## #SPI接口 
- SPI(Serial Peripheral Interface -- 穿行总线)总线系统是一种同步串行外设接口，它可以使MCU与各种外围设备以串行方式进行通信以交换信息。SPI总线可直接与各个厂家生产的多种标准外围器件相连，包括FLASHRAM、网络控制器、LCD显示驱动器、A/D转换器和MCU等。该接口一般使用4条线：串行时钟线（SCLK）、主机输入/从机输出数据线MISO、主机输出/从机输入数据线MOSI和低电平有效的从机选择线NSS。
- SPI接口的全称是"Serial Peripheral Interface"，意为串行外围接口,是Motorola首先在其MC68HCXX系列处理器上定义的。SPI接口主要应用在EEPROM、FLASH、实时时钟、AD转换器，还和数字信号处理器和数字信号解码器之间。
- SPI接口是在CPU和外围低速器件之间进行同步串行数据传输，在主器件的移位脉冲下，数据按位传输，高位在前，低位在后，为全双工通信，数据传输速度总体来说比I2C总线要快，速度可达到几Mbps。
- 特点：信号线少，协议简单，相对数据速率高。
	- （1）MOSI – 主器件数据输出，从器件数据输入
	- （2）MISO – 主器件数据输入，从器件数据输出
	- （3）SCLK –时钟信号，由主器件产生,最大为fPCLK/2，从模式频率最大为fCPU/2
	- （4）NSS – 从器件使能信号，由主器件控制,有的IC会标注为CS(Chip select)
- 在点对点的通信中，SPI接口不需要进行寻址操作，且为全双工通信，显得简单高效。在多个从器件的系统中，每个从器件需要独立的使能信号，硬件上比I2C系统要稍微复杂一些。
- SPI接口在内部硬件实际上是两个简单的移位寄存器，传输的数据为8位，在主器件产生的从器件使能信号和移位脉冲下，按位传输，高位在前，低位在后。如下图所示，在SCLK的上升沿上数据改变，同时一位数据被存入移位寄存器。

## #JavaSPI  
- Java SPI(Service Provider Interface) 是一种用于扩展框架的机制. 它允许开发人员定义一个接口, 然后提供多个实现, 并且不需要知道具体实现类的名字, 而是通过配置文件来指定. Java SPI 机制是一种松耦合的设计模式, 可以帮助开发人员在不修改原始代码的情况下扩展应用程序的功能. 
- Java SPI 的使用方法如下:
  1. 定义接口: 首先定义一个接口, 作为扩展点. 
  2. 实现接口: 编写多个实现类来实现这个接口.
  3. 配置文件: 在METEA-INF/services 目录下, 创建一个以接口权路径名命名的文件, 文件中每行写一个实现类的全路径名.
  4. 加载实现类: 使用Java SPI 机制, 通过ServiceLoader.load() 方法加载实现类.
- 以下是一个简单的Java SPI 示例:
	- 定义接口: 
	```java
	public interface HelloService {
	    void sayHello();
	}
```
	
	 - 实现接口:
	```java
	public class EnglishHelloServiceImpl implements HelloService {
	    public void sayHello() {
	        System.out.println("Hello!");
	    }
	}
	
	public class ChineseHelloServiceImpl implements HelloService {
	    public void sayHello() {
	        System.out.println("你好！");
	    }
	}
```

	 - 配置文件
	   在 META-INF/services 目录下创建一个名为 `com.example.HelloService` 的文件，内容如下：
		```properties
		com.example.EnglishHelloServiceImpl
		com.example.ChineseHelloServiceImpl
```
	
	 - 加载实现类
	```java
	ServiceLoader<HelloService> serviceLoader = ServiceLoader.load(HelloService.class);
	for (HelloService service : serviceLoader) {
	    service.sayHello();
	}
```

	- 输出结果:
	```java
	Hello!
	你好！
```

