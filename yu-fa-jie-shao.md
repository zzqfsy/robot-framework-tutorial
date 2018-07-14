# 语法介绍

[RobotFramework官方用户指南](http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html)

#### 注意点

* 每一行说一句话
* 4个空格很重要

#### 测试脚本模板

```text
*** Settings ***
​
*** Variables ***
​
*** Test Cases ***
​
*** Keywords ***
​
```

#### 语法分析

Settings、Variables、Test Cases、Keywords是测试脚本的4大结\(jin\)构\(gang\)。

**Settings**

引入类库\(Library\)：我们可以使用官方提供的自建库\(BuiltIn\)、官方扩展库\(Collections、DateTime、Dialogs...\)、自定义库\(robotframework\_redislibrary、robotframework\_databaselibrary...\)。

类库的一般用法：

```text
*** Settings ***
Library    Collections
​
*** Test Cases ***
Collections Example2
    ${List}=    Create List
    Append To List    ${List}    a
    Log    ${List}
    ${varList}=    Create List    a
    should be equal    ${List}    ${varList}
```

引入资源\(Resource\)：可以引入其他的Keywords.robot文件，来包装关键字，供其他测试用例使用。

资源的一般用法：

myList.robot

```text
*** Settings ***
Library    Collections
​
*** Keywords ***
Collections A
    ${List}=    Create List
    Append To List    ${List}    a
    Log    ${List}
```

test.robot

```text
*** Settings ***
Resource    myList.robot
​
*** Test Cases ***
Collections Example2
    Collections A
```

**Variables**

Python支持的类型，RF也都能支持。

变量的一般用法：

```text
*** Variables ***
${x}    我是变量
```

**Test Cases**

测试集，由多个测试用例组合测试。

用例的一般用法：

```text
*** Test Cases ***
Test Case 1
    Log    我是用例
```

**Keywords**

关键字，用于组装语句，一般组装为一件事。

关键字的一般用法：

```text
*** Test Cases ***
Today's Date
    ${date}=    Dinner    Mr.sun    Ms.qian
    Log    ${date}

*** Keywords ***
Dinner
    [Arguments]    ${gentleman}    ${lady}
    [Return]    ${gentleman} and ${lady} have dinner
```

