---
layout:     post                    # 使用的布局（不需要改）
title:      越来越感觉Gambit对复杂网格设计力不从心               # 标题 
subtitle:   ICEM	 #副标
date:       2020-04-18              # 时间
author:     nanmu                      # 作者
header-img: img/post-bg-coffee.jpeg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - 学习
---
### 越来越感觉Gambit对复杂网格设计力不从心
一直有一个三维的复杂模型，因为Gambit的划分过于繁琐，导致拖延了大半年
故决心重拾ICEM
* **ICEM怎么打开**
时隔多年，又重新回到ANSYS的目录文件里找ICEM，因为现在用的电脑上的ANSYS包是整个拷过来的，开始菜单里并没有ICEM的运行程序，导致找不到打开ICEM的方法。
* **尝试一**
找到ICEM目录，发现各种杂七杂八的exe文件一大堆，完全找不到启动的主程序
```
.\ANSYS Inc\v150\icemcfd
```
* **尝试二**
Workbench里找到ICEM cfd，试图从这里打开
![image.png](https://upload-images.jianshu.io/upload_images/22945609-9b974b015dc83309.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
却被告知找不到文件
![image.png](https://upload-images.jianshu.io/upload_images/22945609-dda1e9778b30e17d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
考虑一番，可能ICEM启动主程序为一个bat批处理文件，怀疑在某次垃圾清理的时候被360误杀了
遂上网寻找，正好发现小木虫上有人发了批处理文件代码，虽然遇到的并不是同一个问题
```
@echo off

rem set ICEM_LICENSING_FLAGS=ansys
rem set ICEM_LICENSING_FLAGS=icem
rem set ICEM_LICENSE_FILE=c:\icemcfd\5.1-win\lic\license.dat
rem set ANSYSLMD_LICENSE_FILE=c:\icemcfd\5.1-win\lic\license.dat
rem if "%ICEM_ACN%"=="" set ICEM_ACN=%~dp0..
set sub=\bin\
set full=%~dp0
call set work=%%full:%sub%=%%
set ICEM_ACN=%work%

set aie_mode=1
rem No default - determine during initialization
rem if "%AI_ENV_PRODUCT%" == "" set AI_ENV_PRODUCT=fsi
rem Need to allow user to hard set AI_ENV_PRODUCT for PRO/E External
rem Analysis to work properly.

rem Since bat files set the variables in the calling msdos prompt window if you
rem run icemcfd -ai and then icemcfd -4 you will not get the -4 GUI.  So reset the
rem variables here to clean out old values.  -Wayne

set ICEM_AI_ENVIRONMENT=
set AI_ENV_CFD=
set AI_ENV_CFD_ONLY=
set AI_ENV_CFX=
set AI_NO_BLOCKING=
set AI_NO_POST=
set AI_ENV_CART3D=
set AI_ENV_LSTC=
set AI_ENV_ANSYS_SOLVERS=

if not exist "%ANSYS81_DIR%" set ANSYS81_DIR=%ICEM_ACN%\bin
if not exist "%ANSYS90_DIR%" set ANSYS90_DIR=%ICEM_ACN%\bin
if not exist "%ANSYS100_DIR%" set ANSYS100_DIR=%ICEM_ACN%\bin
if not exist "%ANSYS110_DIR%" set ANSYS110_DIR=%ICEM_ACN%\bin
if not exist "%ANSYS120_DIR%" set ANSYS120_DIR=%ICEM_ACN%\bin
if not exist "%ANSYS121_DIR%" set ANSYS121_DIR=%ICEM_ACN%\bin
if not exist "%ANSYS130_DIR%" set ANSYS130_DIR=%ICEM_ACN%\bin

set PRINT_MESSAGE=
if "%ANS_FLEXLM_DEBUG%"=="1" set PRINT_MESSAGE=1
if "%ANS_FLEXLM_DEBUG%"=="2" set PRINT_MESSAGE=1

set VAR1=
set FLAVOR=
set AI_ENV_TEST_LICENSING=

set VAR1=%~1
if "%PRINT_MESSAGE%"=="1" echo VAR1 is %VAR1%

if "%VAR1%" == "-4" goto top
rem if "%VAR1%" == "-batch" goto top
rem if "%VAR1%" == "-script" goto top
rem if "%VAR1%" == "-app" goto top
if "%VAR1%" == "-ai" goto top
if "%VAR1%" == "-cfd" goto top
if "%VAR1%" == "-cfx" goto top
if "%VAR1%" == "-ansys" goto top
if "%VAR1%" == "-fsi" goto top
if "%VAR1%" == "-lstc" goto top
if "%VAR1%" == "-autohex" goto top
if "%VAR1%" == "-batchsurf" goto top

rem move call for test_licensing to initialization stage
set AI_ENV_TEST_LICENSING=1

:top
if "%~1"=="" goto bot

if "%~1"=="-4" (
    set aie_mode=0
    set AI_ENV_PRODUCT=cfd
    shift
    goto top
)
rem if "%~1"=="-app" (
rem     set AI_ENV_PRODUCT=%~2
rem     shift
rem     shift
rem     goto top
rem )
if "%~1"=="-cfd" (
    set FLAVOR=-cfd
    if "%~2"=="-no_hexa" set AI_NO_BLOCKING=1
    if "%~3"=="-no_hexa" set AI_NO_BLOCKING=1
    if "%~4"=="-no_hexa" set AI_NO_BLOCKING=1
    if "%~2"=="-no_post" set AI_NO_POST=1
    if "%~3"=="-no_post" set AI_NO_POST=1
    if "%~4"=="-no_post" set AI_NO_POST=1
    if "%~2"=="-cart3d" set AI_ENV_CART3D=1
    if "%~3"=="-cart3d" set AI_ENV_CART3D=1
    if "%~4"=="-cart3d" set AI_ENV_CART3D=1
    set AI_ENV_PRODUCT=cfd
    shift
    goto top
)
if "%~1"=="-cfx" (
    set FLAVOR=-cfx
    if "%~2"=="-no_hexa" set AI_NO_BLOCKING=1
    if "%~3"=="-no_hexa" set AI_NO_BLOCKING=1
    if "%~2"=="-no_post" set AI_NO_POST=1
    if "%~3"=="-no_post" set AI_NO_POST=1
    set AI_ENV_PRODUCT=cfx
    shift
    goto top
)
if "%~1"=="-ansys" (
    set FLAVOR=-ansys
    if "%~2"=="-no_hexa" set AI_NO_BLOCKING=1
    if "%~3"=="-no_hexa" set AI_NO_BLOCKING=1
    if "%~2"=="-no_post" set AI_NO_POST=1
    if "%~3"=="-no_post" set AI_NO_POST=1
    set AI_ENV_PRODUCT=cfx
    shift
    goto top
)
if "%~1"=="-ai" (
    set FLAVOR=-ai
    if "%~2"=="-no_hexa" set AI_NO_BLOCKING=1
    if "%~3"=="-no_hexa" set AI_NO_BLOCKING=1
    if "%~2"=="-no_post" set AI_NO_POST=1
    if "%~3"=="-no_post" set AI_NO_POST=1
    set AI_ENV_PRODUCT=fea2.0
    shift
    goto top
)
if "%~1"=="-fsi" (
    set FLAVOR=-fsi
    if "%~2"=="-no_hexa" set AI_NO_BLOCKING=1
    if "%~3"=="-no_hexa" set AI_NO_BLOCKING=1
    if "%~4"=="-no_hexa" set AI_NO_BLOCKING=1
    if "%~2"=="-no_post" set AI_NO_POST=1
    if "%~3"=="-no_post" set AI_NO_POST=1
    if "%~4"=="-no_post" set AI_NO_POST=1
    if "%~2"=="-cart3d" set AI_ENV_CART3D=1
    if "%~3"=="-cart3d" set AI_ENV_CART3D=1
    if "%~4"=="-cart3d" set AI_ENV_CART3D=1
    set AI_ENV_PRODUCT=fsi
    shift
    goto top
)
if "%~1"=="-lstc" (
    set FLAVOR=-lstc
    if "%~2"=="-no_post" set AI_NO_POST=1
    if "%~3"=="-no_post" set AI_NO_POST=1
    set AI_ENV_PRODUCT=lstc
    shift
    goto top
)

if "%~1" == "-autohex" goto AUTOHEXA

if "%~1" == "-batchsurf" goto BATCHSURF

if "%~1" == "-h" goto UsageCFD
if "%~1" == "-help" goto UsageCFD
if "%~1" == "/?" goto UsageCFD

:bot

if "%PRINT_MESSAGE%"=="1" echo FLAVOR is %FLAVOR%

if "%~1"=="-no_hexa" set AI_NO_BLOCKING=1
if "%~2"=="-no_hexa" set AI_NO_BLOCKING=1
if "%~3"=="-no_hexa" set AI_NO_BLOCKING=1
if "%~1"=="-no_post" set AI_NO_POST=1
if "%~2"=="-no_post" set AI_NO_POST=1
if "%~3"=="-no_post" set AI_NO_POST=1
if "%~1"=="-cart3d" set AI_ENV_CART3D=1
if "%~2"=="-cart3d" set AI_ENV_CART3D=1
if "%~3"=="-cart3d" set AI_ENV_CART3D=1

if "%FLAVOR%"=="-cfd" (
    set AI_ENV_PRODUCT=cfd
    goto bot1
)
if "%FLAVOR%"=="-cfx" (
    set AI_NO_POST=1
    set AI_ENV_ANSYS_SOLVERS=1
    set AI_ENV_PRODUCT=cfx
    goto bot1
)
if "%FLAVOR%"=="-ansys" (
    set AI_NO_POST=1
    set AI_ENV_ANSYS_SOLVERS=1
    set AI_ENV_PRODUCT=ansys
    goto bot1
)
if "%FLAVOR%"=="-ai" (
    set AI_ENV_PRODUCT=fea2.0
    goto bot1
)
if "%FLAVOR%"=="-fsi" (
    set AI_ENV_PRODUCT=fsi
    goto bot1
)
if "%FLAVOR%"=="-lstc" (
    set AI_ENV_PRODUCT=lstc
    goto bot1
)

:bot1

if "%AI_NO_BLOCKING%"=="0" set AI_NO_BLOCKING=
if "%AI_NO_POST%"=="0" set AI_NO_POST=
if "%AI_ENV_CART3D%"=="0" set AI_ENV_CART3D=

if "%PRINT_MESSAGE%"=="1" echo AI_NO_BLOCKING is %AI_NO_BLOCKING%
if "%PRINT_MESSAGE%"=="1" echo AI_NO_POST is %AI_NO_POST%
if "%PRINT_MESSAGE%"=="1" echo AI_ENV_CART3D is %AI_ENV_CART3D%

if "%aie_mode%"=="1" (
    set ICEM_AI_ENVIRONMENT=1
    if "%AI_ENV_PRODUCT%"=="fea2.0" (
        set AI_ENV_ANSYS=1
        set AI_ENV_LSDYNA=1
        set AI_ENV_ABAQUS=1
        set AI_ENV_AUTODYN=1
    )
    if "%AI_ENV_PRODUCT%"=="cfd" (
        set AI_ENV_CFD=1
        set AI_ENV_CFD_ONLY=1
    )
    if "%AI_ENV_PRODUCT%"=="cfx" (
        set AI_ENV_CFD=1
        set AI_ENV_CFD_ONLY=1
        set AI_ENV_CFX=1
        set AI_ENV_PRODUCT=cfd
    )
    if "%AI_ENV_PRODUCT%"=="ansys" (
        set AI_ENV_CFD=1
        set AI_ENV_PRODUCT=fea2.0
        set AI_ENV_ANSYS=1
        set AI_ENV_LSDYNA=1
        set AI_ENV_AUTODYN=1
    )
    if "%AI_ENV_PRODUCT%"=="fsi" (
        set AI_ENV_CFD=1
        set AI_ENV_PRODUCT=fea2.0
        set AI_ENV_ANSYS=1
        set AI_ENV_LSDYNA=1
        set AI_ENV_ABAQUS=1
        set AI_ENV_AUTODYN=1
    )
    if "%AI_ENV_PRODUCT%"=="commonstruct" (
        set AI_ENV_ANSYS=1
        set AI_ENV_LSDYNA=1
        set AI_ENV_ABAQUS=1
        set AI_ENV_AUTODYN=1
    )  
    if "%AI_ENV_PRODUCT%"=="lstc" (
        set AI_ENV_CFD=1
        set AI_ENV_CFD_ONLY=1
        set AI_ENV_LSTC=1
        set AI_ENV_PRODUCT=cfd
    )
)

if "%PRINT_MESSAGE%"=="1" echo AI_ENV_PRODUCT is %AI_ENV_PRODUCT%

if not exist Uninst.isu goto StartCFD
if exist icemcfd.bat goto WrongDir

:StartCFD
if not exist "%ICEM_ACN%\bin\med.exe" goto NoInstall

if "%PROCESSOR_ARCHITECTURE%"=="x86" (
    set AWP_FRAMEWORK_PLATFORM=Win32
) else if "%PROCESSOR_ARCHITECTURE%"=="AMD64" (
    set AWP_FRAMEWORK_PLATFORM=Win64
) else (
    echo.Error: Environment variable PROCESSOR_ARCHITECTURE should contain
    echo.either x86 or AMD64
    Pause
    exit /b 1
)

set KEEP_PATH=%PATH%
set PATH=%ICEM_ACN%\bin;%ICEM_ACN%\lib;%PATH%;%ICEM_ACN%\toolswin32
if exist "%AWP_ROOT130%\AISOL\Bin\%ANSYS_SYSDIR%" set PATH=%PATH%;%AWP_ROOT130%\AISOL\Bin\%ANSYS_SYSDIR%
if exist "%AWP_ROOT130%\Framework\Bin\%AWP_FRAMEWORK_PLATFORM%" set PATH=%PATH%;%AWP_ROOT130%\Framework\Bin\%AWP_FRAMEWORK_PLATFORM%

if "%~1" == "-batch" goto StartBatch

rem med.exe %* | "%ICEM_ACN%\toolswin32\cat.exe"
start /wait med.exe %*
goto StartFinish

:AUTOHEXA
set AUTOHEX_ROOT=%ICEM_ACN%
if not exist "%AUTOHEX_ROOT%\bin\autohex.exe" goto NoInstall
set KEEP_PATH=%PATH%
set PATH=%ICEM_ACN%\bin;%ICEM_ACN%\lib;%PATH%;%ICEM_ACN%\toolswin32
"%AUTOHEX_ROOT%\bin\autohex.exe"
goto StartFinish

:BATCHSURF
set ICEM_AI_ENVIRONMENT=1
set AI_ENV_ANSYS=1
set AI_ENV_LSDYNA=1
set AI_ENV_ABAQUS=1
set AI_ENV_AUTODYN=1
set AI_ENV_PRODUCT=fea2.0
set ICEM_SCRIB=%ICEM_ACN%\lib\scrib
set TCL_LIBRARY=%ICEM_ACN%\lib\tcl8.3.3
set TK_LIBRARY=%ICEM_ACN%\lib\tk8.3.3
if not exist "%ICEM_ACN%\bin\wish.exe" goto NoInstall
set KEEP_PATH=%PATH%
set PATH=%ICEM_ACN%\bin;%ICEM_ACN%\lib;%PATH%;%ICEM_ACN%\toolswin32
"%ICEM_ACN%\bin\wish.exe" "%ICEM_SCRIB%\app\main.tcl" app_bsm_generic"
goto StartFinish

:StartBatch
start /b /wait med_batch.exe %*

:StartFinish
set PATH=%KEEP_PATH%
goto ExitCFD

:WrongDir
echo Can't run ICEM CFD in the Installation Directory!
pause
goto ExitCFD

:NoInstall
echo Can't find the Installation Directory!
pause
goto ExitCFD

:UsageCFD
echo Usage: icemcfd.bat [-script ScriptName] [-4] [-app APP] [-cfd] [-batch] [projectfile]
pause

:ExitCFD
```
保存到txt，改后缀bat，放入
```
.\ANSYS Inc\v150\icemcfd\win64_amd\bin
```
* **好，ICEM复活**
