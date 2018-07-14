# Web测试案例

这里以百度搜索为例
封装关键字module.robot
```
*** Settings ***
Library    Selenium2Library

*** Keywords ***
baidu search
    [Arguments]    ${search_key}
    open browser    https://www.baidu.com    gc
    input text    id=kw    ${search_key}
    click button    id=su
#    close browser
```
测试用例baidu.robot
```
*** Settings ***
Resource    module.robot

*** Test Cases ***
se_case1
    baidu search    金刚腿
se_case2
    baidu search    铁头功
se_case3
    baidu search    金刚腿铁头功
```
运行结果
```
robot baidu.robot
```
![selenium_example1](https://zzqfsy.github.io/image/robotframework/selenium_example1.png)

