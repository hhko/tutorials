## 참고 자료
- [Secure WinRM for Ansible (Certificates) in 10 Steps [How-To]](https://adamtheautomator.com/ansible-winrm/)
- [Introducing PSRemoting Linux and Windows : How to Guide](https://adamtheautomator.com/psremoting-linux/#Connecting_tofrom_WindowsLinux_with_Password_Authentication)
- [WinRM 세팅 101](https://gist.github.com/ajchemist/5ae3b87add56d39a5b051d860b8bc781)
- [HTTP를 사용하도록 WinRM 구성](https://docs.vmware.com/kr/vRealize-Orchestrator/8.8/com.vmware.vrealize.orchestrator-use-plugins.doc/GUID-D4ACA4EF-D018-448A-866A-DECDDA5CC3C1.html)


## 윈도우 -(로컬)-> 윈도우
```
Set-Service -Name "WinRM" -StartupType Automatic
Start-Service WinRM
Enable-PSRemoting -SkipNetworkProfileCheck -Force
Set-Item WSMan:\localhost\Client\TrustedHosts -Force -Value *
Invoke-Command -Computername 192.168.100.103 -ScriptBlock { dir c:\ }
```

## 윈도우 -(원격)-> 윈도우
```shell
# 윈도우 제어 서버
Set-Service -Name "WinRM" -StartupType Automatic
Start-Service WinRM
Enable-PSRemoting -SkipNetworkProfileCheck -Force
Set-Item WSMan:\localhost\Client\TrustedHosts -Force -Value *

# 윈도우 클라이언트
Set-Service -Name "WinRM" -StartupType Automatic
Start-Service WinRM
Enable-PSRemoting -SkipNetworkProfileCheck -Force
Set-Item WSMan:\localhost\Client\TrustedHosts -Force -Value *
$creds = Get-Credential
Invoke-Command -Computername 192.168.100.103 -ScriptBlock { dir c:\ } -Authentication Negotiate -Credential $creds
```
