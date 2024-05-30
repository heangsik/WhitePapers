# WSL 설치

## 수동설치

- Windows PowerShell 관리자 권한으로 실행

  - 입력

    > dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

    > dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

  - 필요시 재부팅
    > wsl --set-default-version 2 <-- 입력

- [마이크로소프트 공식 설치법](https://learn.microsoft.com/ko-kr/windows/wsl/install-manual#downloading-distributions) 링크에서 설치 하고자 하는 OS 다운
  ![다운파일](../image/wsl_ubuntu.jpg)
- 다운 받은 파일 설치
