---
description: 我们这是一个实用型的电子书，每节都会有些练习，帮助理解flink。
---

# 1.4 基础练习

## 练习工程介绍

### 基本环境

* 要求
  * 用Java 8或Java 11的Java JDK
  * git
  * IDE(IntelliJ推荐)
* 克隆并构建[https://github.com/ververica/flink-training](https://github.com/ververica/flink-training)
* 按照[`README.md`](https://github.com/ververica/flink-training/tree/master#readme)说明来操作。
* 默认情况下，Scala处于禁用状态。要使用Scala，请在gradle.properties中更新此设置：

```
Org.gradle.project t.Enable_Scala=True
```

* 将此项目导入到您的IDE中...

### 工程结构

![](<../../../.gitbook/assets/image (8) (1).png>)

* 文件夹部分：前3个是公共的，后面每个文件夹是一个主题练习
  * common：example、exercise/common(主要是数据类型、生成数据的工具、通用utils（生成数据，地理位置工具）)、公共test
  * config/checkstyle：略
  * gradle/wrapper：gradle打包
  * 下面4个文件夹分别代表一个主题练习，后面分别讲解。
* 下面文件部分为工程自带文件及gradle属性文件，按需编辑即可。

### 尝试运行工程

通过运行测试来验证它是否正常工作，例如，

Org.apache.flink.training.exercises.ridecleansing.RideCleansingIntegrationTest

或者通过运行一个例子，例如，

Org.apache.flink.training.examples.ridecount.RideCountExample

### 一个主题练习的结构

![](<../../../.gitbook/assets/image (3).png>)

* src下一共有3层，
  * main: 代码框架已经写好，但是跟该练习相关的核心实现部分需要读者实现
  * solution：完整实现
  * test：单元测试或者整功能测试，测试驱动开发
* readme.md:对该练习的背景和要点描述，做之前需要好好研读。

## 数据集介绍

练习中使用数据[生成器(generators)](https://github.com/ververica/flink-training/blob/master/common/src/main/java/org/apache/flink/training/exercises/common/sources)产生模拟的事件流。 该数据的灵感来自[纽约市出租车与豪华礼车管理局(New York City Taxi & Limousine Commission)](http://www.nyc.gov/html/tlc/html/home/home.shtml) 的公开[数据集](https://uofi.app.box.com/NYCtaxidata)中有关纽约市出租车的车程情况。

## TaxiRide data

出租车数据集包含有关纽约市个人出租车的车程信息。

每次乘坐出租车都有两个事件: START 事件(其中 isStart 为 TRUE)和 END 事件(其中 isStart 为 FALSE)。

每个事件都由十一个字段组成：

```
rideId         : Long      // a unique id for each ride
taxiId         : Long      // a unique id for each taxi
driverId       : Long      // a unique id for each driver
isStart        : Boolean   // TRUE for ride start events, 
                           //   FALSE for ride end events
eventTime      : Instant   // timestamp
startLon       : Float     // the longitude of the ride start location
startLat       : Float     // the latitude of the ride start location
endLon         : Float     // the longitude of the ride end location
endLat         : Float     // the latitude of the ride end location
passengerCnt   : Short     // number of passengers on the ride
```

## TaxiFare data

```
rideId         : Long      // 每次车程的唯一id
taxiId         : Long      // 每一辆出租车的唯一id
driverId       : Long      // 每一位司机的唯一id
isStart        : Boolean   // 行程开始事件为 TRUE， 行程结束事件为 FALSE
eventTime      : Instant   // 事件的时间戳
startLon       : Float     // 车程开始位置的经度
startLat       : Float     // 车程开始位置的维度
endLon         : Float     // 车程结束位置的经度
endLat         : Float     // 车程结束位置的维度
passengerCnt   : Short     // 乘车人数
```

每个 TaxiRide 事件都有一个相匹配的 TaxiFare 事件和该乘坐的支付数据。TaxiFare 记录包含与匹配的 TaxiRide 记录相同的 rideId、 taxId 和 driverId 值。

请注意，虽然付款是在一次乘坐结束时，这些出租车费记录的时间戳是当乘坐开始的时间。



## 测试驱动

* 大多数练习都有单元测试和端到端集成测试，例如,
  * rideCleansingUnitTest
  * RideCleansingIntegrationTest
* 开箱即用，测试默认通过
  * 因为他们默认测试 RideCleansingSolution
  * 直到你把这行代码从exercise中移除（throw）

```
private static class NYCFilter implements FilterFunction<TaxiRide> {
​
    @Override
    public boolean filter(TaxiRide taxiRide) throws Exception {
        throw new MissingSolutionException();
    }
}
```

对于行程清洗练习，单元测试测试filter功能，而集成测试测试整个数据处理管道。

这个项目中的测试用于测试每个Exercise类，如果一个练习抛出 MissingSolutionException（不改代码的情况下），那么它们会吞下这个异常，并将测试应用到相应的 Solution 类。



{% hint style="info" %}
重要说明：为方便部分读者使用，本人维护一个配套的工程，详细链接：TODO。

1、将官方工程中的项目管理工具由gradle替换成maven。

2、仅提供java版本代码，不提供scala代码实现。

3、仅提供main下面的解决方案，通过注释将//throw new MissingSolutionException()；这一行注掉，然后填上我们自己要实现的内容；不另外包含solution目录（相当于是将main和solution合并）。
{% endhint %}
