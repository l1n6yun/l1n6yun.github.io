---
title: Windows 开启3389.bat
date: 2016-12-24 12:27:20
tags: [Windows,3389]
categories: 

---
```bat

@echo off
cls
:index
title 开启3389批处理工具     
color B
cls
echo                     ╭─────────────────╮
echo   ╭────────┤           ├────────╮
echo   │                ╰─────────────────╯                │
echo   │                            ≮开启系统终端≯                          │
echo   │                                                                      │
echo   │    A.查看当前系统 B.开启XP,2003终端服务 C.开启window 2000终端服务 │
echo   │                                                                      │
echo   │    D.终端端口修改        R.重新启动计算机       Q.退出批处理         │
echo   │                                                                      │
echo   │                                                                      │
echo   │                                                                      │
echo   ╰───────────────────────────────────╯
echo.
set start=
set /p start=    请选择:
if "%start%"=="a" goto ck
if "%start%"=="b" goto xp
if "%start%"=="c" goto 20
if "%start%"=="d" goto xg
if "%start%"=="e" goto cq
if /i "%start%"=="q" goto end
goto index
:ck
set a="XP"
set b="server 2003"
set d="WINNT"
type c:\boot.ini|findstr %a%>nul&&echo 当前操作为XP系统
type c:\boot.ini|findstr %b%>nul&&echo 当前操作为2003系统
dir c:\|find %d%>nul&&echo 当前操作为2000系统
pause
goto index
:xp
if exist "c:\shit.reg" (del c:\shit.reg
echo Windows Registry Editor Version 5.00>>c:\shit.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]>>c:\shit.reg
echo "fDenyTSConnections"=dword:00000000>>c:\shit.reg
regedit /s c:\shit.reg
sc config TermService start= demand
sc start TermService
del c:\shit.reg
)
echo Windows Registry Editor Version 5.00>>c:\shit.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]>>c:\shit.reg
echo "fDenyTSConnections"=dword:00000000>>c:\shit.reg
regedit /s c:\shit.reg
sc config TermService start= demand
sc start TermService
del c:\shit.reg
pause
goto index
:20
if exist "c:\shift.reg" (del c:\shift.reg
echo Windows Registry Editor Version 5.00 >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\netcache] >>c:\shift.reg
echo "Enabled"="0" >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon]>>c:\shift.reg
echo "ShutdownWithoutLogon"="0" >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Installer] >>c:\shift.reg
echo "EnableAdminTSRemote"=dword:00000001 >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server] >>c:\shift.reg
echo "TSEnabled"=dword:00000001 >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\TermDD] >>c:\shift.reg
echo "Start"=dword:00000002 >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\TermService] >>c:\shift.reg
echo "Start"=dword:00000002 >>c:\shift.reg
echo [HKEY_USERS\.DEFAULT\Keyboard Layout\Toggle] >>c:\shift.reg
echo "Hotkey"="1" >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\Wds\rdpwd\Tds\tcp] >>c:\shift.reg
echo "PortNumber"=dword:00000D3D >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp] >>c:\shift.reg
echo "PortNumber"=dword:00000D3D >>c:\shift.reg
regedit /s c:\shift.reg
sc config TermService start= demand
sc start TermService
)
echo Windows Registry Editor Version 5.00 >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\netcache] >>c:\shift.reg
echo "Enabled"="0" >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon] >>c:\shift.reg
echo "ShutdownWithoutLogon"="0" >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Installer] >>c:\shift.reg
echo "EnableAdminTSRemote"=dword:00000001 >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server] >>c:\shift.reg
echo "TSEnabled"=dword:00000001 >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\TermDD] >>c:\shift.reg
echo "Start"=dword:00000002 >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\TermService] >>c:\shift.reg
echo "Start"=dword:00000002 >>c:\shift.reg
echo [HKEY_USERS\.DEFAULT\Keyboard Layout\Toggle] >>c:\shift.reg
echo "Hotkey"="1" >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\Wds\rdpwd\Tds\tcp] >>c:\shift.reg
echo "PortNumber"=dword:00000D3D >>c:\shift.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp] >>c:\shift.reg
echo "PortNumber"=dword:00000D3D >>c:\shift.reg
regedit /s c:\shift.reg
sc config TermService start= demand
sc start TermService
del c:\shift.reg
pause
goto index
:xg
set /p 源数=    修改终端端口号为:
set /a 源数=%源数% || goto :eof
:dosomething
set /a 余数 = %源数% %% 16
set /a 源数 /= 16
call :转换 %余数%
set 余数=%ret%
set 计算结果=%余数%%计算结果%
if %源数% lss 16 goto end
goto dosomething
:转换
set ret=
if "%1" == "10" set ret=A
if "%1" == "11" set ret=B
if "%1" == "12" set ret=C
if "%1" == "13" set ret=D
if "%1" == "14" set ret=E
if "%1" == "15" set ret=F
if %1 lss 10 set ret=%1
goto :eof
:end
call :转换 %源数%
set 源数=%ret%
if "%源数%" == "0" set 源数=
echo 0x%源数%%计算结果% >%windir%\temp\shift.txt
set ret=
set 源数=
set 余数=
set 计算结果=
for /f "delims=" %%a in ('type %windir%\temp\shift.txt') do (
set j=%%a
)
set jd=%j:~3%
if exist "%windir%\temp\window.reg" (del %windir%\temp\window.reg
echo Windows Registry Editor Version 5.00>%windir%\temp\window.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\Wds\rdpwd\Tds\tcp]>>%windir%\temp\window.reg
echo "PortNumber"=dword:00000%jd%>>%windir%\temp\window.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp]>>%windir%\temp\window.reg
echo "PortNumber"=dword:00000%jd%>>%windir%\temp\window.reg
regedit /s %windir%\temp\window.reg
)
echo Windows Registry Editor Version 5.00>%windir%\temp\window.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\Wds\rdpwd\Tds\tcp]>>%windir%\temp\window.reg
echo "PortNumber"=dword:00000%jd%>>%windir%\temp\window.reg
echo [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp]>>%windir%\temp\window.reg
echo "PortNumber"=dword:00000%jd%>>%windir%\temp\window.reg
regedit /s %windir%\temp\window.reg
del %windir%\temp\window.reg
@echo                      修改终端端口成功
pause
goto index
:cq
shutdown /r /t 0
```