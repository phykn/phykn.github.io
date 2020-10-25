---
title:  "Jupyter Notebook 가상환경 연결"
date: 2020-10-25
categories: [Code]
search: true
---

### 시작 폴더 변경하기
Jupyter Notebook 시작 위치는 config 파일에서 지정할 수 있다.

##### 1. config 파일 생성
콘솔 창에서 generate-config 명령어로 jupyter_notebook_config.py 파일을 생성한다.
```powershell
(base) C:\> jupyter notebook --generate-config
```
##### 2. 시작 위치 설정
jupyter_notebook_config.py 파일에 다음 줄을 추가해 준다. 여기서는 D:\ 로 설정
```python
c.NotebookApp.notebook_dir = 'D:\\'
```
윈도우의 경우 `\` 대신 `\\`를 입력