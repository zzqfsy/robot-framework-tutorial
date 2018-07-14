# 简易案例

#### 运行RIDE {#运行RIDE}

打开控制台\(命令提示符\)，输入ride.py运行

> 需要将\?\Python27\Scripts加入至环境变量

弹出RIDE IDE：  
 ![RIDE](https://zzqfsy.github.io/image/robotframework/ride.png)

#### 简单的测试集 {#简单的测试集}

```text
*** Settings ***

*** Variables ***

*** Test Cases ***
First test case
    Begin web test

Second test case
    End web test

*** Keywords ***
Begin web test
    Log This is first test case

End web test
    Log HelloWorld
```

#### 运行 {#运行}

> 这个测试集运行了两个测试用例\(First test case、Second test case\)

运行结果：  
 ![RIDE\_RUN](https://zzqfsy.github.io/image/robotframework/ride_run.png)

