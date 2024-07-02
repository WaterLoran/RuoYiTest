1. # 概况

   前华为测试开发工程师, 充分借鉴httprunner框架的设计优点 和 华为某大部门的测试框架的规范, 首创基于aop实现的面向切面编程描述式的python语法框架. 超级容易上手, 自动化最优实践!!

    特点是, 易读易维护, 我希望这个框架的设计思路能够统一未来新增的自动化框架市场. 当前该框架有其直白高效的范式, 数据和逻辑高度解耦, 核心功能有日志, 报告, 断言, 提取信息, 多次重试, 插件功能有执行mysql命令, excel断言, 脚本连跑执行入口等. 

   

2. # 相关框架设计思路及其优势

   这个框架主要分为接口自动化和WebUI自动化两部分. 接口自动化部分, 三层解耦, 即业务逻辑描述层, 框架逻辑层, 接口数据描述层. WebUI部分, 三层解耦, 即业务描述层, 框架逻辑层, page数据描述层. 总体的设计思路为, 定义好业务脚本层的描述范式 和 底层数据的描述方式(接口的描述和page的描述), 然后再由切面编程的方式, 由逻辑层去解析上层和下层的描述信息, 然后调用驱动模块来执行相关代码. 这样子设计的好处是, 代码重复率低, 容易阅读, 上手容易, 不需要掌握复杂的框架逻辑和Python语法. 

   其中WebUI框架部分更是短短的400多行代码就完成了框架核心

   

   ### 相关功能

   核心部分: 日志, 报告, 请求, 配置, 断言, 提取信息
   扩展功能: excel断言功能

   扩展部分: 执行入口, 发送报告

   

   ### 相关成就

   1. 实习生可以在没有python基础的前提下, 快速上手编写接口关键字和接口脚本, UI的page和对应脚本, 上手难度超低
   2. 目前5star, 3fork

   

   ### 痛点解决

   **接口自动化部分**

   1. 接口的的封装仅保留url, 请求方法, 请求体, 而不需要其他繁琐的代码
   2. 业务脚本层为使用函数式编程, 支持用户直观的描述业务行为而不用考虑python的语法, 代码难度极低
   3. 日志能够去根据用例路径归档到本地

   

   **WebUi自动化部分**

   1. 使用seleniumbase之后, 不用考虑driver的适配, 不用考虑显隐性等待, 执行过程机器稳定

   2. page的封装极其简洁, 仅需要定位信息和元素的描述信息, 不需要调用原始的seleniumAPI来做各种定位查找点击输入等操作

   3. 业务脚本层采用函数式变成, 相当于调用参数并传参, 极其简洁

      

   ### 框架扩展支持度

   1. 对于每个测试步骤都有一个上下文管理器, 在新增功能的时候, 直接在线性的处理逻辑中取出上下文信息, 然后处理完后, 更新到上下文中去即可
   2. 采用切面编程的思路, 将通用逻辑统一到装饰器中(@Api.josn等)去, 后续对功能的扩展极其方便

   

3. # 如何搭建并运行

   a. fork仓库并gitclone下来, 或者直接download下来

   b. 使用pycharm打开, 并创建虚拟环境

   c. 找到setup.py阅读, 并编译Python包, 拷贝到虚拟环境中去安装

   d. B站搜索 罗兰水, 并找到安装若依管理系统的视频, 并安装
       视频地址: https://www.bilibili.com/video/BV1394y1E7Ak/?vd_source=dfda50881c9c583b748c882e99f0fa21

   e. pycharm工程配置pytest

   f. 使用pip install -r requirements.txt 来安装需要的第三方包(待提供)

   g.上Bilibili搜索 罗兰水 找到 "RuoyiTest测试框架上手视频", 视频地址: https://www.bilibili.com/video/BV1ZJ4m1E77S/?vd_source=dfda50881c9c583b748c882e99f0fa21
   
4. # 使用的技术方案及第三方库

   公共的库: Python, pytest, pyyaml, logging, allure, easydict, pytest-check

   接口自动化方面: request, jsonpath

   WebUI自动化方面: seleniumbase

   

5. # 如何编写一个用例

   1. 修改配置文件, 即修改config目录下的environment文件, 包括URL和账号密码

   2. 调试通登录逻辑, 可在调试脚本的时候再去调试

   3. 按照关键字的格式去封装关键字

   4. 使用关键字来组合成自动化脚本, 并调试

      

6. # 相关配套工具,及其说明

   后期将会上传excel功能用例文件转自动化脚本的工具 , 或许可能会上传录制转脚本的工具(不打算上传主要是用处不大不好用)

   

7. # 各个目录说明

   cases: 用于存放用例的目录

   cases/api: 用于存放API业务脚本

   cases/web_ui: 用于存放web_Ui的业务脚本

   common: 用于存放API关键字, 下面的目录是按照web上的功能模块来拆分的

   config: 配置目录, 目录下的__init__文件用于读取并初始化配置, 其他目录是配置, 比如user.yaml是一些冷数据

   core: 框架核心的目录

   core/logic: api框架的核心, 包含的功能有读取关键字数据, 入参填充, 发请求, 默认断言, 提取响应, 日志等功能

   core/page: web_Ui框架的核心, 包含页面page信息的读取, 解释执行信息, 调用seleniumbase进行执行, 断言等功能

   core/ruoyi_hook: 整个框架的日志功能, 将在钩子函数中注册日志器, 绑定句柄, 将日志打印到控制台, 文件, 和推送到

   httpserver(目前未追加该功能)

   files: 用于存放文件, 业务脚本中部分接口会涉及文件的上传, 文件同意存放在这个目录

   pages: 用于存放page信息, 即不同page的各个元素的定位和描述信息的组合

   utils: 工具组合目录

   run_api_case.py 执行入口

8. # 功能说明

   ## 核心功能-日志功能

   1. 支持打印到控制台, 和文件
   2. 支持根据测试文件的路径信息打印到对应的路径

   

   ## 核心功能-报告

   

   ## 核心功能-配置

   1. 支持用yaml来存放冷数据(即长期不会变的数据, 理论上是不会变更的数据), 并且初始化好后, 支持以config.xxa.xxb的方式来调用该数据

   

   ## 核心功能-API断言

   

   ## 核心功能-提取响应信息

   1. 支持使用jsonpath到响应体中去提取信息, 例如fetch=[reg, "data_id", "$.data.id"], 描述为使用"$.data.id" 这个表达式去响应中提取信息, 并存放reg下data_id变量中.

   2. 支持在一个响应体中提取多个信息, 表达式为

      ```
      fetch=[
      	[reg, "name", "$.data.name"],
      	[reg, "id", "$.data.id"],
      ]
      ```

      即用二维列表来描述提取信息的说明

   ## 扩展功能-excel断言功能

   参考test_excel_assert_001脚本, 注意被用于断言的文件需要放在files目录下, 一般用法为先将文件下载到files目录下, 然后接着去断言

   
   
9. # 进一步的深入的提纲内容, 技术群信息

   添加微信: 13538302392 并备注Ruoyitest, 即可拉入群聊沟通学习