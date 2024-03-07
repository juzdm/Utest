# 嵌入式如何写UT

## UT框架

- **c++/c** 
  - gtest：Android系统标准UT框架https://github.com/google/googletest

- **c**
  - UNITY:  专门给嵌入式领域设计的UT框架https://www.throwtheswitch.org/unity

- c#
  - [NUnit](https://www.lambdatest.com/blog/nunit-vs-xunit-vs-mstest/#NUnit)
  - [XUnit](https://www.lambdatest.com/blog/nunit-vs-xunit-vs-mstest/#XUnit)
  - [MSTest](https://www.lambdatest.com/blog/nunit-vs-xunit-vs-mstest/#MSTest)

## 嵌入式的特点

- 资源受限

  受限的memory、cpu、磁盘空间大小等硬件资源

- 硬件依赖

  依赖外部硬件资源，比如依赖外部总线的访问等等。

## UT分类

### 在host端运行单元测试

#### 缺点

- 需要使用不同的编译工具链

  一般来说，嵌入式使用的是arm平台，host使用的是x86，架构不一样，所以运行测试需要不同的编译工具链，对代码架构设计要求更高

- mock工作量较大，代码直接的接口依赖需要特别清晰

  如果在host端运行ut，则mock的接口会多，所以工作量大一些，而且要求维护模块间接口的尽量稳定

#### 优点

- 不依赖嵌入式设备运行
- 没有资源限制
- 运行方便

### 在设备端运行单元测试

#### 缺点

- 嵌入式设备上运行的代码需要编译额外的测试框架
- 资源限制
- 设备依赖

#### 优点

- 可测试的维度比较深，mock的相对较少

- 在CICD集成有难度，CICD需要通过qemu模拟器或者连接真实设备去运行测试

  

## 优秀实践

### Kunit

Kunit 是kernel的单元测试框架

https://www.kernel.org/doc/html/latest/dev-tools/kunit/architecture.html#kunit-tool-command-line-test-harness

### Android 

https://source.android.com/docs/core/tests/development/atest?hl=zh-cn

## 困难点

- 硬件依赖

  嵌入式设备一般依赖一些硬件资源，比如某些特有的总线或者设备、硬件接口等等，对这部分进行测试难点较大，所以需要对这部分进行fake，或者基于qemu等模拟器进行测试。

举例 1：

​      Android Automotive os 是android 给车载领域定制的os，Android Automotive 底层通过can总线访问和车控系统进行交互，如果真的对Android Automotive 进行测试，需要有实车环境，要求比较苛刻，所有 Android Automotive 专门基于qemu模拟器提供了一套can总线fake的实现，然后就可以在PC段通过qemu测试 Android Automotive framework和app层的程序。

- 交叉编译

  一般来说，嵌入式使用的是arm平台，host使用的是x86，架构不一样，所以如果需要在host端进行测试，需要添加x86编译支持

举例 2：

​    智能电表终端通过i2c接口与电表通信，读取电表中的数据，其中电表终端进行电表管理、费用计算与统计等模块业务功能复杂，需要进行充分的测试。所以对这部分核心代码进行了重构，保证这部分代码和底层I2c库的接口稳定。然后fake掉i2c库的实现，然后对整个工程添加x86编译工具链的支持，这样就可以在PC端通过gtest框架测试核心逻辑。

## Nuint
- 历史悠久，是一个非常成熟的框架
- 丰富的扩展插件和广泛的社区支持。
- 支持并行运行test，效率更高
