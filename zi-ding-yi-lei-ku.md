# 自定义类库

### 自定义类库

自定义类库可以帮助我们方便的开发出自己想要的功能，只要是Python支持的功能，都能实现相应功能的系统关键字。这里以Redis连接类库\(robotframework-redislibrary\)举例

#### 安装

```text
pip install robotframework-redislibrary
```

#### 阅读源码

```text
cd \?\Python27\Lib\site-packages\robotframework-redislibrary-?
```

通过目录文件可以看出真正执行的脚本源码在目录\?\Python27\Lib\site-packages\RedisLibrary.

打开目录下的\*.py文件，截取一段代码分析：RedisLibraryKeywords.py

```text
# -*- coding: utf-8 -*-
from robot.api import logger
from robot.api.deco import keyword
from version import VERSION
import redis
​
__author__ = 'Traitanit Huangsri'
__email__ = 'traitanit.hua@gmail.com'
__version__ = VERSION
​
class RedisLibraryKeywords(object):
​
    @keyword('Connect To Redis')
    def connect_to_redis(self, redis_host, password, redis_port=6379, db=0): # pragma: no cover
        """Connect to the Redis server.
​
        Arguments:
            - redis_host: hostname or IP address of the Redis server.
            - redis_port: Redis port number (default=6379)
            - db: Redis keyspace number (default=0)
​
        Return redis connection object
​
        Examples:
        | ${redis_conn}= | Connect To Redis | redis-dev.com | 6379 |
        """
        try:
            redis_conn = redis.StrictRedis(host=redis_host, port=redis_port, db=db, password=password)
        except Exception as ex:
            logger.error(str(ex))
            raise Exception(str(ex))
        return redis_conn
```

注解@keyword\('Connect To Redis'\)定义了测试用例的关键字，方法的参数对应于关键字后面的几个参数。

注释也有4部分组成，方法描述、参数、返回值、RF使用例子。

其他代码与常规Python一致。

包描述文件\(**init**.py\)：

```text
# -*- coding: utf-8 -*-
from RedisLibraryKeywords import RedisLibraryKeywords
from version import VERSION
​
__author__ = 'Traitanit Huangsri'
__email__ = 'traitanit.hua@gmail.com'
__version__ = VERSION
​
class RedisLibrary(RedisLibraryKeywords):
    ROBOT_LIBRARY_SCOPE = "GLOBAL"
    ROBOT_LIBRARY_DOC_FORMAT = "ROBOT"
```

标识了包package的作用标识了当前版本、作者信息导入RedisLibrary，即实例化RedisLibraryKeywordsROBOT\_LIBRARY\_SCOPE：

> TEST CASEA new instance is created for every test case. A possible suite setup and suite teardown share yet another instance. This is the default.TEST SUITEA new instance is created for every test suite. The lowest-level test suites, created from test case files and containing test cases, have instances of their own, and higher-level suites all get their own instances for their possible setups and teardowns.GLOBALOnly one instance is created during the whole test execution and it is shared by all test cases and test suites. Libraries created from modules are always global.来源： [http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html\#using-return-setting](http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html#using-return-setting)

ROBOT\_LIBRARY\_DOC\_FORMAT

> Starting from Robot Framework 2.7.5, library documentation tool Libdoc supports documentation in multiple formats. If you want to use something else than Robot Framework's own documentation formatting, you can specify the format in the source code using ROBOT\_LIBRARY\_DOC\_FORMAT attribute similarly as scope and version are set with their own ROBOT\_LIBRARY\_\* attributes.The possible case-insensitive values for documentation format are ROBOT \(default\), HTML, TEXT \(plain text\), and reST \(reStructuredText\). Using the reST format requires the docutils module to be installed when documentation is generated.Setting the documentation format is illustrated by the following Python and Java examples that use reStructuredText and HTML formats, respectively. See Documenting libraries section and Libdoc chapter for more information about documenting test libraries in general.来源： [http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html\#specifying-documentation-format](http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html#specifying-documentation-format)

#### 自己的类库

我们尝试模范robotframework-redislibrary，来构建自己的类库

\?\Python27\Lib\site-packages\ZzqfsyDateTimeLibrary\**init**.py

```text
# -*- coding: utf-8 -*-
from ZzqfsyDateTimeLibrary import ZzqfsyDateTimeLibrary
from version import VERSION
​
__author__ = 'zzqfsy'
__email__ = 'zzqfsy@gmail.com'
__version__ = VERSION
​
class datetimeLibrary(ZzqfsyDateTimeLibrary):
    ROBOT_LIBRARY_SCOPE = "GLOBAL"
    ROBOT_LIBRARY_DOC_FORMAT = "ROBOT"
```

\?\Python27\Lib\site-packages\ZzqfsyDateTimeLibrary\version.py

```text
#!/usr/bin/env python
# -*- coding: utf-8 -*-
​
VERSION = "0.1"
```

\?\Python27\Lib\site-packages\ZzqfsyDateTimeLibrary\ZzqfsyDateTimeLibrary.py

```text
# -*- coding: utf-8 -*-
# datetime library
​
from robot.api import logger
from robot.api.deco import keyword
from version import VERSION
import time
import datetime
​
__author__ = 'zzqfsy'
__email__ = 'zzqfsy@gmail.com'
__version__ = VERSION
​
class ZzqfsyDateTimeLibrary(object):
    @keyword('Get Now Timestamp')
    def get_now_timestamp(self, coefficient=1000):
        """ Get now timestamp, Default coefficient is the millisecond level
​
        Arguments:
​
        Examples:
        | ${data}= | Get Now Timestamp |
        | ${data}= | Get Now Timestamp | #{coefficient} |
        """
        return int(time.time() * coefficient)
​
    @keyword('Timestamp To TimeString')
    def timestamp_to_timeString(self, timestamp):
        """ Parse timestamp to TimeString
​
        Arguments:
​
        Examples:
        | ${data}= | Timestamp To TimeString | ${timestamp}
        """
        coefficient = len(str(timestamp)) - 10
        if coefficient > 0:
            timestamp = timestamp / (10**coefficient)
        timeArray = time.localtime(timestamp)
        return time.strftime("%Y-%m-%d %H:%M:%S", timeArray)
​
if __name__ == '__main__':
    timeLib = ZzqfsyDateTimeLibrary()
    timestamp = timeLib.get_now_timestamp()
    print(timestamp)
    timeStr = timeLib.timestamp_to_timeString(timestamp)
    print timeStr
    print timeLib.timestamp_to_timeString(1527786061000)
```

编写测试用例

```text
*** Settings ***
Library ZzqfsyDateTimeLibrary
​
*** Test Cases ***
ZzqfsyDateTimeLibrary Example1
    ${timestamp}=    Get Now Timestamp
    ${timestring}=    Timestamp To TimeString    ${timestamp}
```

运行输出

```text
Starting test: testDemo.Test.ZzqfsyDateTimeLibrary Example1
20180711 15:23:09.546 : INFO : ${timestamp} = 1531293789546
20180711 15:23:09.548 : INFO : ${timestring} = 2018-07-11 15:23:09
Ending test: testDemo.Test.ZzqfsyDateTimeLibrary Example1
```

