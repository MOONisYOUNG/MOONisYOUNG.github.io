---
layout: single
title: "WSL2 / VirtualBox 충돌 방지 Tip"
excerpt: "💥 Hyper-V 활성화 여부를 꼭 체크하자!"
categories:
  - os
tags:
  - Linux
  - WSL2
  - Virtual-Box
  - PowerShell
---
윈도우 컴퓨터에서 Linux를 사용하기 위해 고려하는 방식으로 크게 3가지 있습니다.
1. 멀티 부팅 세팅
2. VirtualBox 설치
3. WSL 설치

  

저는 아직 Linux 경험치가 부족하다고 판단한 관계로, 처음부터 1번을 택하기에는 리스크가 있다고 생각했습니다.  
특히 Linux는 윈도우 환경과 다르게 휴지통 기능이 없기에... 자칫 중요한 파일을 날리면 먹통 컴퓨터가 될 수 있다는 불안감이 앞섰습니다.  

  

따라서 저는 상황에 따라 VirtualBox와 WSL을 바꿔 가면서 쓰기로 결정했습니다!  
개인적인 Linux 실험이 필요할 때에는 WSL를 쓰고, 여러 우분투 버전 비교가 필요할 때에는 VirtualBox를 사용하기로 했습니다.  

  

그런데 두 기능을 오갈 때, Hyper-V 설정 여부를 고려해야 할 경우가 많더라구요.  
자주 쓰게 될 명령문들이라 오래 기억하기 위해 여기에 적어 놓게 되었습니다 ^_^  
+) 하단의 명령문들은 윈도우 PowerShell에서 동작합니다!  

```powershell
# Hyper-V 활성화 여부 확인하는 법
bcdedit | find "hypervisorlaunchtype"

# Hyper-V 비활성화 (VirtualBox 사용 시)
bcdedit /set hypervisorlaunchtype off

# Hyper-V 활성화 (WSL 사용 시)
bcdedit /set hypervisorlaunchtype auto
```

Hyper-V와 VirtualBox는 하드웨어 가상화 기술을 사용하는 '하이퍼바이저'에 해당합니다.  
이 둘은 같은 하드웨어 가상화 기술을 사용하는데, 이 기술은 하나의 하이퍼바이저에서만 동작한다고 합니다.  
따라서 한 쪽 하이퍼바이저에서 비활성으로 설정해야, 다른 한 쪽의 하이퍼바이저를 사용할 수 있는 구조라고 하네요!  

  

저는 이게 스위치 또는 시소 같은 관계라고 느껴지더라구요.  
아주 흥미로웠습니다~  
  
### 📑 참고 자료 링크
* <a href="https://aproid.github.io/2023/07/17/virtualbox-turtle-fix/" target="_blank">VirtualBox 속도 향상 방법 (feat. WSL2)</a>
* <a href="https://primi.tistory.com/22" target="_blank">WSL vs. Virtual Machine 차이점 비교</a>
