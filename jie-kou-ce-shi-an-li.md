# 接口测试案例

这里以X包利率为例
测试用例xbao.robot
```
*** Settings ***
Library     RequestsLibrary

*** Test Cases ***
Get YuEBao Interest Rate
    Create Session    yuebao    https://yebprod.alipay.com
    ${resp}=    Get Request    yuebao    /yeb/iframe/qiriIframe.htm
    Should Be Equal As Strings    ${resp.status_code}    200
    ${ret}=    Convert To String    ${resp.content}
    Log    ${ret}
    ${interest_rate}=    Should Match Regexp    ${ret}    var\\s*Data=\\[(.*)\\]
    Log    ${interest_rate}

*** Keywords ***

```
运行结果
```
robot xbao.robot
```
![xbao](https://zzqfsy.github.io/image/robotframework/xbao.png)
