# SD2-启动bash文件分析

## 通过命令：.\webui-user.bat 运行

## .\web-user.bat

```bash
@echo off

set PYTHON=
set GIT=
set VENV_DIR=
set COMMANDLINE_ARGS=

call webui.bat

```

可见，运行：.\web-user.bat的本身还是运行 webui.bat 命令。

## .\webui.bat

```bash

@echo off

@REM 加载设置bat文件 
if exist webui.settings.bat (
    call webui.settings.bat
)

@REM 设置相关环境变量
if not defined PYTHON (set PYTHON=python)
if defined GIT (set "GIT_PYTHON_GIT_EXECUTABLE=%GIT%")
if not defined VENV_DIR (set "VENV_DIR=%~dp0%venv")

@REM 这是启动与临时重启
set SD_WEBUI_RESTART=tmp/restart
@REM 设置不报告错误
set ERROR_REPORTING=FALSE

@REM 创建临时目录
mkdir tmp 2>NUL

@REM 在临时目录中启动/stdout.txt
%PYTHON% -c "" >tmp/stdout.txt 2>tmp/stderr.txt
@REM 设置错误等级
if %ERRORLEVEL% == 0 goto :check_pip
@REM 不能运行Python则跳转至show_stdout_stderr
echo Couldn't launch python
goto :show_stdout_stderr

@REM 检查pip是否安装
:check_pip
%PYTHON% -mpip --help >tmp/stdout.txt 2>tmp/stderr.txt
@REM 设置错误0则跳转至 start_venv
if %ERRORLEVEL% == 0 goto :start_venv
@REM 设置如果pip本地安装器不存在，则跳转至show_stdout_stderr
if "%PIP_INSTALLER_LOCATION%" == "" goto :show_stdout_stderr
@REM 通过Python启动pip安装器，安装stderr.txt
%PYTHON% "%PIP_INSTALLER_LOCATION%" >tmp/stdout.txt 2>tmp/stderr.txt
@REM 如果错误0，则跳转至 start_venv
if %ERRORLEVEL% == 0 goto :start_venv
@REM 同时输出不能安装 pip 
echo Couldn't install pip
@REM 跳转至show_stdout_stderr
goto :show_stdout_stderr

@ start_venv 模块
:start_venv
@ 如果环境变量VENV_DIR为-，则跳转至：skip_venv
if ["%VENV_DIR%"] == ["-"] goto :skip_venv
@ 如果环境变量SKIP_VENV为1，则跳转至：skip_venv
if ["%SKIP_VENV%"] == ["1"] goto :skip_venv

@ %VENV_DIR% 目录下使用Python操作 stderr.txt
dir "%VENV_DIR%\Scripts\Python.exe" >tmp/stdout.txt 2>tmp/stderr.txt
@REM 如果错误等级为0，则跳转至：activate_venv
if %ERRORLEVEL% == 0 goto :activate_venv

@REM 遍历并设置PYTHON_FULLNAME
for /f "delims=" %%i in ('CALL %PYTHON% -c "import sys; print(sys.executable)"') do 
@REM 设置PYTHON_FULLNAME
set PYTHON_FULLNAME="%%i"
@REM 输出使用PYTHON_FULLNAME在目录VENV_DIR下创建的环境venv
echo Creating venv in directory %VENV_DIR% using python %PYTHON_FULLNAME%
@REM 使用Python在环境VENV_DIR中运行tmp/stderr.txt
%PYTHON_FULLNAME% -m venv "%VENV_DIR%" >tmp/stdout.txt 2>tmp/stderr.txt
@REM 如果错误等级为0，则跳转至：activate_venv
if %ERRORLEVEL% == 0 goto :activate_venv
@REM 如果无法在VENV_DIR目录中创建环境，则跳转至：show_stdout_stderr
echo Unable to create venv in directory "%VENV_DIR%"
goto :show_stdout_stderr

@REM activate_venv模块
:activate_venv
set PYTHON="%VENV_DIR%\Scripts\Python.exe"
echo venv %PYTHON%

@REM skip_venv模块
:skip_venv
if [%ACCELERATE%] == ["True"] goto :accelerate
goto :launch

@REM accelerate模块
:accelerate
@REM 输出正在加速
echo Checking for accelerate
@REM 设置加速器 ACCELERATE
set ACCELERATE="%VENV_DIR%\Scripts\accelerate.exe"
if EXIST %ACCELERATE% goto :accelerate_launch

@REM launch模块
:launch
@REM 启动launch.py文件
%PYTHON% launch.py %*
@REM 判断如果存在tmp/restart，即已经重启过，则跳转至：skip_venv
if EXIST tmp/restart goto :skip_venv
@REM 暂停->退出
pause 
exit /b

@REM accelerate_launch模块
:accelerate_launch
@REM 输出加速中
echo Accelerating
%ACCELERATE% launch --num_cpu_threads_per_process=6 launch.py
if EXIST tmp/restart goto :skip_venv
pause
exit /b

:show_stdout_stderr

echo.
echo exit code: %errorlevel%

@REM 遍历tmp\stdout.txt变量，设置siz
for /f %%i in ("tmp\stdout.txt") do set size=%%~zi
if %size% equ 0 goto :show_stderr
echo.

@REM 输出标准输出
echo stdout:
type tmp\stdout.txt

@REM show_stderr模块
:show_stderr

@REM 遍历tmp\stderr.txt变量，设置siz
for /f %%i in ("tmp\stderr.txt") do set size=%%~zi
if %size% equ 0 goto :show_stderr
echo.
@REM 输出stderr
echo stderr:
type tmp\stderr.txt

:endofscript

echo.
@REM 否则输出运行不成功
echo Launch unsuccessful. Exiting.
pause

```
