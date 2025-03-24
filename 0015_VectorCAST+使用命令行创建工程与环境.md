# 0015_VectorCAST+使用命令行创建工程与环境


## 第 1 页

 
 
 
 
 
VectorCAST 命令行创建工程与环境 
主题（命令行、自动化） 
 
 
2023-08-07 
作者 
Gu, Jian 
状态 
已发布 
发布范围 
对外 
发布方 
维克多汽车技术（上海）有限公司 
© 2023 版权所有。 
任何分发或复制均须经Vector 中国书面批准。 



## 第 2 页

 
版本: 2023-08-07 
      
2 / 7 
 
主题（命令行、自动化） 
文档管理 
发布 
日期 
姓名 
2023-08-07 
Jgu 
 
 
历史版本 
日期 
修改内容 
2023-08-07 
创建文档（基于VectorCAST2023SP2） 
2024-01-29 
更新创建工程命令，指向CFG 文件创建工程 
2024-05-31 
更新脚本链接，同时优化脚本，自动根据同一路径下的不同env 创建对应环境 
 
 
 



## 第 3 页

 
版本: 2023-08-07 
      
3 / 7 
 
主题（命令行、自动化） 
目录 
1 文章介绍 ................................................................................................................................. 4 
2 方法步骤 ................................................................................................................................. 5 
 



## 第 4 页

 
版本: 2023-08-07 
      
4 / 7 
 
主题（命令行、自动化） 
1 
文章介绍 
本篇文章介绍如何通过命令行的方式创建VectorCAST 工程，并同时采用命令行方式在工程中创建测
试环境，并自动创建测试用例同时获取测试报告； 
 
Demo 脚本请根据如下链接获取: https://portal.vector.com/shared/66c80437-65db-4595-
91aa-f2d59ccb30fd 
➢ 
XXX_.CFG 用于设定编译链等工程配置信息(必要); 
➢ 
Launch.bat 为启动脚本，下载后结合本地信息，更新脚本中的环境变量（必要）； 
➢ 
XXX.env 环境的构建脚本，用于设定环境名称、依赖的路径、覆盖率类型，加桩减桩等配置
（必要）； 
➢ 
report 可用于存放生成的环境报告； 
上述内容要放在同一目录下； 
脚本中的中文注释有助于理解涉及到的命令行的功能作用，请阅读； 



## 第 5 页

 
版本: 2023-08-07 
      
5 / 7 
 
主题（命令行、自动化） 
2 
方法步骤 
VectorCAST 安装目录下的manage.exe 支持工程层面的命令行操作，通过在cmd 终端中输入”manage 
–help”即可查阅各个命令选项作用： 
 
VectorCAST 安装目录下的clicast.exe 支持环境层面的命令行操作，通过在cmd 终端中输入” clicast 
help all”即可查阅各个命令选项作用： 
 
环境变量VECTORCAST_DIR 指VectorCAST 本地安装路径。 
➢ 
Step 1：构建工程 
构建工程命令：  
%VECTORCAST_DIR%\manage --project=%PRJ_NAME% –create 
⚫ 
--project 后指定工程名称 
➢ 
Step 2：根据配置文件构建工程套件： 
%VECTORCAST_DIR%\manage --project=%PRJ_NAME% --cfg-to-compiler=%CFG_NAME%  
⚫ 
--cfg-to-compiler=%CFG_NAME% 指向环境配置文件，其中设定编译、链接等命令 
➢ 
Step3: 基于env 脚本导入构建测试环境： 
%VECTORCAST_DIR%\manage --project=%PRJ_NAME% --testsuite TestSuite --import %ENV_NAME%.env 



## 第 6 页

 
版本: 2023-08-07 
      
6 / 7 
 
主题（命令行、自动化） 
⚫ 
--import %ENV_NAME%.env：通过env 脚本来创建环境，env 用户可自行创建，也可以参考
在每个vcast 工程的environment 目录下，会存放每个环境的脚本，其中的env 脚本即此
处所指的env 脚本，格式相同，用户可编辑，用户设定环境的名称，依赖的头文件以及源
文件路径、覆盖率类型、user code、函数打桩等环境相关内容。 
⚫ 
--testsuite TestSuite：命令中指定一个名为TestSuite 的测试套件，创建的环境会在该
testsuite 节点下（用户可自定义testsuite 名称） 
 
➢ 
Step4：将环境的工作空间迁移至工程的工作空间中 
%VECTORCAST_DIR%/manage --project=%PRJ_NAME% --testsuite TestSuite --migrate %ENV_NAME% 
⚫ 
--migrate 后面指定要移入工程中的环境名称 
➢ 
Step5：编译工程环境 
%VECTORCAST_DIR%\manage --project=%PRJ_NAME% --build 
⚫ 
--build 为编译命令选项 
 
➢ 
Step6：为构建的环境自动添加BASIS PATH 测试用例 
 
%VECTORCAST_DIR%\manage --project=%PRJ_NAME% -e %ENV_NAME% --clicast-args TOols 
AUTO_Test_generation %AUTO_TEST_SCRIPT% 
 
⚫ 
--clicast-args TOols AUTO_Test_generation 命令为自动创建测试用例 
⚫ 
%AUTO_TEST_SCRIPT% 可自定义该环境变量指定存放测试用例的脚本名称 
 
➢ 
Step7：将自动创建的测试用例导入对应的测试环境中 
%VECTORCAST_DIR%\manage --project=%PRJ_NAME% -e %ENV_NAME% --clicast-args TESt Script 
Run %AUTO_TEST_SCRIPT% 
⚫ 
--clicast-args TESt Script Run 该命令为导入测试用例脚本至环境中 
⚫ 
%AUTO_TEST_SCRIPT%指定为存储测试用例的tst 脚本文件(step6 中创建) 
 
➢ 
Step8：编译环境并执行全部的测试用例 
 
%VECTORCAST_DIR%\manage --project=%PRJ_NAME% -e %ENV_NAME%  --build-execute 



## 第 7 页

 
版本: 2023-08-07 
      
7 / 7 
 
主题（命令行、自动化） 
⚫ 
--build-execute 编译环境并执行全部测试用例 
 
➢ 
Step9：获取初步创建好的环境的测试报告 
 
%VECTORCAST_DIR%/manage --project=%PRJ_NAME% -e %ENV_NAME% --clicast-args REports Custom 
Management %REPORT_PATH%/%ENV_NAME%_Management_report.html 
⚫ 
----clicast-args REports Custom Management 用来指定报告类型，例如当前报告类型为
management report 
⚫ 
%REPORT_PATH%/%ENV_NAME%_Management_report.html 该命令用来指定生成的报告位置
以及名称 
通过上述命令可初步创建一个VecotCAST 工程环境，可在此基础上优化测试用例实现测试需求，
对于复杂环境的构建以及测试用例设计等建议采用工具界面方式构建测试环境，便于调试。 


