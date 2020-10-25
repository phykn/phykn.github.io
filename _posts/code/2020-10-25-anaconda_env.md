---
title:  "Anaconda Env 생성, 삭제"
date: 2020-10-25
categories: [Code]
search: true
---

### Anaconda Env
Anaconda Env는 아나콘다 내 가상환경으로 독립적인 환경을 만들어 패키지를 유용하게 관리할 수 있다.

##### 1. Env 생성
```powershell
conda create -n [가상환경 이름] python=[파이썬 버전]
```
예시) 가상환경 이름 = cpu, Python 버전 = 3.6
```powershell
(base) C:\> conda create -n cpu python=3.6
```

##### 2. Env 활성화
```powershell
conda activate [가상환경 이름]
```
예시)
```powershell
(base) C:\> conda activate cpu
(cpu) C:\>
```

##### 3. Env 목록 확인
```powershell
conda env list
```

##### 4. Env 삭제
```powershell
conda env remove -n [가상환경 이름]
```
예시) cpu 가상환경 삭제
```powershell
(base) C:\> conda env remove -n cpu
```