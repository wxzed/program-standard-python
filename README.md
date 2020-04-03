# python编程规范

python语言 Raspberry封库编程规范 <br>
## 大原则：
 * 看到github，就有购买欲望，并且知道怎么购买
 * 看到example，就能玩通，尽量不用去查其他资料
 * 看到函数名称，就能基本猜到函数功能
 * 如果只有一个存取接口，不要写子类，全部在一个类里实现，比如DFRobot_Sensor，也不要有_IIC的后缀
## 细则
 * 寄存器命名和手册保持一致，可以加传感器型号头，例如 DS18B20_REG_CONFIG
 * 行业标准COLOR_RGB565_BLACK,写明编码标准，而不是直接用 COLOR_BLACK ，可以放在全局
 * 非行业标准，每个芯片可能都有差异，如果放在类内部不合适，放在全局要加芯片名称头，例如 DS18B20_SPEED_LEVEL1
 
 
注意：所有的库严禁 copy , 所有的库首次编写时完全靠个人对技术手册的理解，遇到问题解决不了后才能参考别人的代码（必须理解人家的代码然后按照自己的理解解决问题）

## 索引

* [参考库](#参考库)
* [文件结构与命名](#文件结构与命名)
* [常量命名](#常量命名)
* [变量命名](#变量命名)
* [结构体](#结构体)
* [函数命名与注释](#函数命名与注释)
* [类](#类)
* [if-else](#if-else)
* [运算符](#运算符)
* [example中py文件的命令](#example中py文件的命令)
* [example中的入口函数定义](#example中的入口函数定义)
* [Readme中多个参数函数写法](#readme中多个参数函数写法)
* [py文件头部写法](#py文件头部写法)
* [高质量封库细节](#高质量封库细节)

## 参考库

https://github.com/DFRobot/DFRobot_RaspberryPi_Motor <br>
https://github.com/DFRobot/DFRobot_INA219<br>
https://github.com/cdjq/DFRobot_DS3231M <br>

## 文件结构与命名

库可以直接放在 Arduino 封库的根目录下，建立 python 目录用于存放树莓派的驱动和demo <br>
 <br>
文档结构应类似如下，如果不与 Arduino 一起使用，则直接将原本 python 文件夹下的内容放在根目录下: <br>
<pre>

--python:
| --raspberrypi:
| | --DFRobot_Module.py
| | --examples:
| | | --Modele_demo_xxxx.py
| | | --Modele_demo_yyy.py
| | ......
|
| --esp32:
| | --DFRobot_Module.py
| | --examples:
| | | --Modele_demo_xxxx.py
| | | --Modele_demo_yyy.py
| | ......
| |

</pre>

## 例程文件头部写法
```python
  '''
  @file 文件名（不能使用中文）
  如何做这个实验，描述实验步骤（只需要下载程序就能肉眼观测到的简单小实验例如blink，这步可以不写）（不能使用中文）
  @n 实验现象是什么（不能使用中文）
  
  @Copyright   Copyright (c) 2010 DFRobot Co.Ltd (http://www.dfrobot.com)
  @licence   The MIT License (MIT)
  
  @author [XXXXX](XXXX.XXXX@dfrobot.com)
  @url https://github.com/DFRobot/DFRobot_Module
  @version  V1.0
  @date  2020-2-12
  '''
```

## 类与注释

类名应当以 DFRobot_ 作为开头, 模组型号作为结尾。<br>
在构造函数中说明参数:
```python
class DFRobot_Module:

  def __init__(i2cAddr):
    ''' Class constructor

    :param i2cAddr    Module's i2c address
    '''
    pass
```

## 常量命名

C 库中的宏，包括枚举变量在 python 中需要写在类的全局变量中，全部使用大写字母和下划线组合

C 库中的定义：
```cpp
#define MODULE_CONF   0x00

typedef enum {
  eModuleConfEnum1,
  eModuleConfEnum2
} eModuleConf_t;
```

对应 python 中的定义：
```py
class DFRobot_Module:

  _REG_CONF = 0x00

  CONF_ENUM1 = 0x00
  CONF_ENUM2 = 0x01

```

## 变量命名
受保护的变量（对应 cpp 里的 protected ）名前需要加 _, 私有的变量（对应 cpp 里的 private ）名前需要加 __ <br>
任何类型的变量和函数名都使用下划线加小写字母命名规则 <br>

例：
```python
class DFRobot_Module:

  _protect_var = 1  # 受保护的变量
  _protect_list = [0, 1]  # 变量注释
  _protect_dict = {
    "a": 1
  }

  __private_var = 1  # 私有的变量
  __private_list = [0, 1]  # 变量注释
  __private_dict = {
    "a": 1
  }

  public_var = 1  # 公有变量
  public_list = [0, 1]
  public_dict = {
    "a": 1
  }
```

## 结构体

python中使用结构体的话，需要通过_fields_属性来定义结构体的构成
```python
class TestStruct(Structure):
    _fields_ = [('x',c_ubyte),
                ('y',c_int),
                ('z',c_double)]
```

## 函数命名与注释
受保护的函数（对应 cpp 里的 protected ）名前需要加 _, 私有的函数（对应 cpp 里的 private ）名前需要加 __ <br>
函数名使用下划线加小写字母命名规则 <br>

示例：
```py
class DFRobot_Module:

  def __init__(self, param1):
    ''' Module init

    :param param1:int Set to ...
    '''
    pass

  def _private_func(self, param1):
    ''' func detail

    :param param1:int Set to ...
    '''
    pass

  def public_func(self, param1):
    ''' Check param1

    :param param1:int Parameter to check

    :return:bool Check result
      :retval True Check succeed
      :retval False Check falied
    '''
    if param1:
      return True
    else:
      return False

  POWER_ON = 0x00
  POWER_OFF = 0x01

  def public_func2(self):
    ''' Return power status

    :return Power status
    '''
    return self._powerStatus

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
## example中py文件的命名
看到demo名称基本就能猜到功能，专有名词之间以下划线连接。
```python
  get_time_and_temp.py
  get_time_from_NTP.py
```

## example中的入口函数定义
```python
    if __name__ == "__main__":
        main()
```

## Readme中多个参数函数写法

采用doxygen注释规则描述参数
```python
  '''
  @brief Set alarm clock
  @param alarmType:EverySecond,
  @n               SecondsMatch,
  @n               SecondsMinutesMatch,
  @n               SecondsMinutesHoursMatch,
  @n               SecondsMinutesHoursDateMatch,
  @n               SecondsMinutesHoursDayMatch, #Alarm1
  @n               EveryMinute,
  @n               MinutesMatch,
  @n               MinutesHoursMatch,
  @n               MinutesHoursDateMatch,
  @n               MinutesHoursDayMatch,        #Alarm2
  @n               UnknownAlarm
  @param days    Alarm clock Day (day)
  @param hours   Alarm clock Hour (hour)
  @param mode:   H24hours, AM, PM
  @param minutes Alarm clock (minute)
  @param seconds Alarm clock (second)
  '''
  set_alarm(alarmType, date, hour, mode, minute, second)
```

## 高质量封库细节
1. 与其他库兼容好
2. 库和 demo 中的便于理解的注释充足
3. 代码功能设置合理，初级 demo（小白用户上手测试文件）用户体验好，中高级 demo （非小白用户）用户体验尽量好
4. 代码简洁精炼，易于升级维护，不是技术难度越高越好，而是以用户用起来简单功能容易上手、有深入功能、api用户体验设计非常好为核心
