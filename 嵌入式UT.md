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
