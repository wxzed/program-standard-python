# 编程规范

python语言 Raspberry封库编程规范 <br>
## 大原则：
 * 看到github，就有购买欲望，并且知道怎么购买
 * 看到ino，就能玩通，尽量不用去查其他资料
 * 看到函数名称，就能基本猜到函数功能
 * 如果只有一个存取接口，不要写子类，全部在一个类里实现，比如DFRobot_Sensor，也不要有_IIC的后缀
## 细则
 * 寄存器命名和手册保持一致，可以加传感器型号头，例如 DS18B20_REG_CONFIG
 * 行业标准COLOR_RGB565_BLACK,写明编码标准，而不是直接用 COLOR_BLACK ，可以放在全局
 * 非行业标准，每个芯片可能都有差异，如果放在类内部不合适，放在全局要加芯片名称头，例如 DS18B20_SPEED_LEVEL1
 
 
注意：所有的库严禁 copy , 所有的库首次编写时完全靠个人对技术手册的理解，遇到问题解决不了后才能参考别人的代码（必须理解人家的代码然后按照自己的理解解决问题）

## 索引

* [参考库](#参考库)
* [变量及函数共同属性](#变量及函数共同属性)
* [变量命名](#变量命名)
* [函数命名](#函数命名)
* [寄存器](#寄存器)
* [结构体](#结构体)
* [枚举](#枚举)
* [列表](#列表)
* [类](#类)
* [if-else](#if-else)
* [运算符](#运算符)
* [Readme中多个参数函数写法](#readme中多个参数函数写法)
* [py文件头部写法](#py文件头部写法)
* [高质量封库细节](#高质量封库细节)

## 参考库

https://github.com/cdjq/DFRobot_DS3231M <br>

## 变量及函数共同属性

变量和函数共同属性
```python
    '''当非下划线开头时代表为公有成员变量'''
    SensorValue        = 0x00
    
    '''当以下划线开头时代表为私有成员变量'''
    _SensorValue        = 0x00
    
    '''当非下划线开头时代表为公有函数'''
    def test_func(self):
        #do something
    
    '''当以下划线开头时代表为私有函数'''
    def _test_func(self):
        #do something
```
## 变量命名
```python
    '''驼峰方式，公有成员变量'''
    SensorValue        = 0x00
    
    '''驼峰方式，私有成员变量'''
    _SensorValue        = 0x00
```

## 函数命名
```python
    '''全部小写，专有名词之间以下划线连接，公有函数'''
    def test_func(self):
        #do something
    '''全部小写，专有名词之间以下划线连接，私有函数'''
    def _test_func(self):
        #do something
```

## 寄存器
寄存器变量命名方式
```python
    '''全部大写，专有名词之间以下划线连接，公有成员变量'''
    REG_RTC_SEC        = 0x68
    
    '''全部大写，专有名词之间以下划线连接，私有成员变量'''
    _REG_RTC_SEC        = 0X00
    
```

## 结构体

python中使用结构体的话，需要通过_fields_属性来定义结构体的构成
```python
class TestStruct(Structure):
    _fields_ = [('x',c_ubyte),
                ('y',c_int),
                ('z',c_double)]
```

## 枚举
枚举变量命名方式
```python
    '''驼峰方式命名，公有成员变量'''
    SquareWave_1Hz      = 0x00
    SquareWave_1kHz     = 0x08
    MinutesMatch                 = 0x07
    MinutesHoursMatch            = 0x08

    '''驼峰方式命名，私有成员变量'''
    _SquareWave_1Hz     = 0x00
    _SquareWave_1kHz    = 0x08
    _MinutesMatch                 = 0x07
    _MinutesHoursMatch            = 0x08
```

## 列表
列表相关命名方式
```python
    '''全部小写，专有名词之间以下划线连接，公有成员变量'''
    test_list          =[]
    month              =[]
    '''全部小写，专有名词之间以下划线连接，公有成员变量'''
    _test_list          =[]
    _month              =[]
```

## 类

以DFRobot_开头,后面接ClassName,ClassName首字母要大些
正确的写法
```python
class DFRobot_Sensor:
```

## if-else
```python
    if mode is True:
        #do something
    else:
        #do something
```


## 运算符

前后需要留空格(此条根据具体情况，不强制)

```python
    x = 0
    x += 1
    x > 10
```

为了各个库之间的互相兼容，必须做到以下几点。<br>

  1. 库的 py 文件中除个别情况外不能有全局变量，所有相关变量应定义在 class 中
  2. 库相关的变量定义要么定义在 class 中，要么加上库名作为前缀
  3. 所有有特殊需求的外设通信（比如说要把IIC通信速率提高到800KHZ），在每次开始通信前必须进行相关配置，通信完成后必须把更改了的配置恢复到默认状态


## Readme中多个参数函数写法

采用doxygen注释规则描述参数
```
    '''
    @brief Set operational mode to VL53L0X
    
    @param mode  Work mode settings
        Single : Single mode
        Continuous : Back-to-back mode
    @param precision  Set measurement precision
        High：High precision(0.25mm)
        Low: Low precision(1mm)
    @return  true if execute successfully, false otherwise.
    '''
    setMode(mode, precision);
```

## py文件头部写法
```python
    '''
    @file 文件名（不能使用中文）
    如何做这个实验，描述实验步骤（只需要下载程序就能肉眼观测到的简单小实验例如blink，这步可以不写）（不能使用中文）
    @n 实验现象是什么（不能使用中文）
    
    @copyright   Copyright (c) 2010 DFRobot Co.Ltd (http://www.dfrobot.com)
    @licence     The MIT License (MIT)
    @author      Alexander(ouki.wang@dfrobot.com)
    @version  V1.0
    @date  2019-07-16
    @get from https://www.dfrobot.com
    @url https://github.com/DFRobot/DFRobot_PAJ7620U2
    '''
```

## 高质量封库细节
