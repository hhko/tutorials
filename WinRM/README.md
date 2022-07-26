## 참고 자료
- https://ploz.tistory.com/m/entry/9-windows-winrm을-이용한-ansible-사용
- https://adamtheautomator.com/python-winrm/
- https://docs.vmware.com/kr/vRealize-Orchestrator/8.8/com.vmware.vrealize.orchestrator-use-plugins.doc/GUID-2F7DA33F-E427-4B22-8946-03793C05A097.html

<br/>

- [PowerShell Remoting from Linux Centos 7 to Windows](https://blog.yucas.net/2021/03/25/powershell-remoting-from-linux-to-windows-centos-7/)
- [Secure WinRM for Ansible (Certificates) in 10 Steps [How-To]](https://adamtheautomator.com/ansible-winrm/)
- [Introducing PSRemoting Linux and Windows : How to Guide](https://adamtheautomator.com/psremoting-linux/#Connecting_tofrom_WindowsLinux_with_Password_Authentication)
- [WinRM 세팅 101](https://gist.github.com/ajchemist/5ae3b87add56d39a5b051d860b8bc781)
- [HTTP를 사용하도록 WinRM 구성](https://docs.vmware.com/kr/vRealize-Orchestrator/8.8/com.vmware.vrealize.orchestrator-use-plugins.doc/GUID-D4ACA4EF-D018-448A-866A-DECDDA5CC3C1.html)
- [Windows 원격 관리](https://runebook.dev/ko/docs/ansible/user_guide/windows_winrm)
- [Ubuntu에 PowerShell 설치](https://docs.microsoft.com/ko-kr/powershell/scripting/install/install-ubuntu?view=powershell-7.2)


## Windows -(Local)-> Windows
```
Set-Service -Name "WinRM" -StartupType Automatic
Start-Service WinRM
Enable-PSRemoting -SkipNetworkProfileCheck -Force
Set-Item WSMan:\localhost\Client\TrustedHosts -Force -Value *
# Self-IP : 127.0.0.1
Invoke-Command -Computername Self-IP -ScriptBlock { dir c:\ }
```

## Windows -(Remote)-> Windows
```shell
# Controlled Windows : 제어되는 윈도우
Set-Service -Name "WinRM" -StartupType Automatic
Start-Service WinRM
Enable-PSRemoting -SkipNetworkProfileCheck -Force
Set-Item WSMan:\localhost\Client\TrustedHosts -Force -Value *

# Controlling Windows : 제어하는 윈도우
Set-Service -Name "WinRM" -StartupType Automatic
Start-Service WinRM
Enable-PSRemoting -SkipNetworkProfileCheck -Force
Set-Item WSMan:\localhost\Client\TrustedHosts -Force -Value *
$creds = Get-Credential
# Target-IP : xxx.xxx.xxx.xxx
Invoke-Command -Computername Target-IP -ScriptBlock { dir c:\ } -Authentication Negotiate -Credential $creds
```
