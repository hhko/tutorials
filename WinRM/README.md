## TODO
- [x] Enable-PSRemoting 상태 확인
  ```shell
  # localhost만 가능하다. 127.0.0.1은 안된다. 자신의 IP 역시 안된다.
  if (-not (Get-PSSessionConfiguration) -or (-not (Get-ChildItem WSMan:\localhost\Listener))) {
    Enable-PSRemoting -SkipNetworkProfileCheck -Force
  }
  ```
- [ ] `Enable-PSRemoting` 반대 명령어?
- [ ] PowserShell 버전 확인
- [ ] 계정 정보 PoserShell 코드화
- [ ] WinRM 인증 타입 이해
  - Basic
  - Certificate
  - Negotiate : Kerberos with an NTLM fallback
  - Kerberos : with no NTLM fallback
  - CredSSP
- [ ] Ubuntu -(포트 오픈 확인)-> Windows
- [ ] Ubuntu PoserShell 패키지 오프라인 설치
- [ ] Ubuntu gss-ntlmssp 패키지 오프라인 설치

## Posershell 명령
- `Get-PSSessionConfiguration`
- `Get-ChildItem`
- `Enable-PSRemoting`
- `Set-Service`
- `Start-Service`
- `Set-Item`
- `Get-Credential`

## 참고 자료
- https://youtu.be/jUgHXBpcRM4
- https://ploz.tistory.com/m/entry/9-windows-winrm을-이용한-ansible-사용
- https://adamtheautomator.com/python-winrm/
- https://docs.vmware.com/kr/vRealize-Orchestrator/8.8/com.vmware.vrealize.orchestrator-use-plugins.doc/GUID-2F7DA33F-E427-4B22-8946-03793C05A097.html
- https://adamtheautomator.com/winrm-ssl/
- https://digitalist.global/article/winrm-ansible/

<br/>

- [PowerShell Remoting from Linux Centos 7 to Windows](https://blog.yucas.net/2021/03/25/powershell-remoting-from-linux-to-windows-centos-7/)
- [Secure WinRM for Ansible (Certificates) in 10 Steps [How-To]](https://adamtheautomator.com/ansible-winrm/)
- [Introducing PSRemoting Linux and Windows : How to Guide](https://adamtheautomator.com/psremoting-linux/#Connecting_tofrom_WindowsLinux_with_Password_Authentication)
- [WinRM 세팅 101](https://gist.github.com/ajchemist/5ae3b87add56d39a5b051d860b8bc781)
- [HTTP를 사용하도록 WinRM 구성](https://docs.vmware.com/kr/vRealize-Orchestrator/8.8/com.vmware.vrealize.orchestrator-use-plugins.doc/GUID-D4ACA4EF-D018-448A-866A-DECDDA5CC3C1.html)
- [Windows 원격 관리](https://runebook.dev/ko/docs/ansible/user_guide/windows_winrm)
- [Ubuntu에 PowerShell 설치](https://docs.microsoft.com/ko-kr/powershell/scripting/install/install-ubuntu?view=powershell-7.2)


## Windows -(Local)-> Windows
```shell
Set-Service -Name WinRM -StartupType Automatic
Start-Service WinRM
Enable-PSRemoting -SkipNetworkProfileCheck -Force
Get-ChildItem WSMan:\localhost\Listener         # winrm e winrm/config/listener
Set-Item WSMan:\localhost\Client\TrustedHosts -Force -Value *
Invoke-Command `
  -Computername 127.0.0.1 `
  -ScriptBlock { cmd.exe /c dir c:\ }
Invoke-Command `
  -Computername 127.0.0.1 `
  -ScriptBlock { powershell.exe Get-ChildItem -Path c:\ }
```

## Windows -(Remote)-> Windows
```shell
#
# Controlled Windows : 제어되는 윈도우
#
Set-Service -Name WinRM -StartupType Automatic
Start-Service WinRM
Enable-PSRemoting -SkipNetworkProfileCheck -Force
Get-ChildItem WSMan:\localhost\Listener         # winrm e winrm/config/listener
Set-Item WSMan:\localhost\Client\TrustedHosts -Force -Value *

#
# Controlling Windows : 제어하는 윈도우
#
Set-Service -Name "WinRM" -StartupType Automatic
Start-Service WinRM
Enable-PSRemoting -SkipNetworkProfileCheck -Force
Get-ChildItem WSMan:\localhost\Listener         # winrm e winrm/config/listener
Set-Item WSMan:\localhost\Client\TrustedHosts -Force -Value *

$creds = Get-Credential
# Target-IP : xxx.xxx.xxx.xxx
Invoke-Command `
  -Computername Target-IP `
  -ScriptBlock { cmd.exe /c dir c:\ } ` 
  -Authentication Negotiate `
  -Credential $creds
Invoke-Command `
  -Computername Target-IP `
  -ScriptBlock { powershell.exe Get-ChildItem -Path c:\ } ` 
  -Authentication Negotiate `
  -Credential $creds
```

## Ubuntu -(Remote)-> Windows
```shell
#
# Controlling Ubuntu : 제어하는 Ubuntu
#

#
# 1. gss-ntlmssp 설치 : https://launchpad.net/ubuntu/+source/gss-ntlmssp/+changelog
#                       gss-ntlmssp/focal,now 0.7.0-4build3 amd64 [installed
#
root@ ... # apt-get install -y gss-ntlmssp 

#
# 2. PoserShell 설치 : https://docs.microsoft.com/ko-kr/powershell/scripting/install/install-ubuntu?view=powershell-7.2
#
# Update the list of packages
sudo apt-get update
# Install pre-requisite packages.
sudo apt-get install -y wget apt-transport-https software-properties-common
# Download the Microsoft repository GPG keys
wget -q "https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/packages-microsoft-prod.deb"
# Register the Microsoft repository GPG keys
sudo dpkg -i packages-microsoft-prod.deb
# Update the list of packages after we added packages.microsoft.com
sudo apt-get update
# Install PowerShell
sudo apt-get install -y powershell
# Start PowerShell
pwsh


```

## FAQ
```
```
