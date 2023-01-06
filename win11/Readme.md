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
