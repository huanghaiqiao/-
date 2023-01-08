
[wsl固定ip方法](https://blog.csdn.net/u011843342/article/details/127581604)

[win11增加打开方式](https://xinzhi.wenda.so.com/a/1661149622200014)

[Win11右键自动展开二级](https://www.xitongzhijia.net/xtjc/20220705/246460.html)

串口通讯到电脑，注意只能连接一个软件

wsl固定ip脚本
```
@echo off
:: 管理员权限运行bat
%1 mshta vbscript:CreateObject("Shell.Application").ShellExecute("cmd.exe","/c %~s0 ::","","runas",1)(window.close)&&exit



@echo off
setlocal enabledelayedexpansion
    :: open wsl
    wsl --exec ls > null
    :: set windows ip
    ipconfig | findstr "172.28.131.102" > nul
    if !errorlevel! equ 0 (
        echo windows ip has set
    ) else (
        netsh interface ip add address "vEthernet (WSL)" 172.28.131.102 255.255.240.0
        echo set windows ip success: 172.28.131.102
    )
 
    :: wsl --shutdown
    wsl -u root ip addr | findstr "172.28.131.101" > nul
    if !errorlevel! equ 0 (
        echo wsl ip has set
    ) else (
        wsl -u root ip addr add 172.28.131.101/20 broadcast 172.17.111.255 dev eth0:1 label eth0:1
        echo set wsl ip success: 172.28.131.101
    )
 
pause
```
wsl映射脚本
```
@echo off
usbipd wsl list
for /f "tokens=1" %%a in ('usbipd wsl list^|findstr JTAG') do (
    set BUSID=%%a
)

for /f "tokens=11" %%a in ('usbipd wsl list^|findstr JTAG') do (
    set STATUS=%%a
)


if "%BUSID%" == "" ( 
    echo 请检查你的JTAG线！！！
) else (
    if "%STATUS%" == "Not" (
        usbipd wsl attach --busid %BUSID%
        usbipd wsl list
    ) else (
        echo Olimex OpenOCD JTAG have Attached
    )
)
mode | findstr COM
echo ------------------
echo 参考一下命令:
echo usbipd wsl list
echo usbipd wsl attach --busid 2-2
echo usbipd wsl detach --all
pause
```
