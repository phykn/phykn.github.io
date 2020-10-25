---
title:  "Jupyter Notebook 가상환경 연결"
date: 2020-10-25
categories: [Code]
search: true
---
Jupyter Notebook 는 기본 (Base) 환경 외 다른 아나콘다의 가상환경을 잡아주지 않는다. 따라서 아래 그림과 같이 다른 가상환경 (Kernel) 에 접근할 수 없다.
<center><img src="/assets/images/code/2020-10-25-jupyter_notebook_kernel_1.png"></center>

### nb_conda 설치
다른 가상환경에 접근하기 위해서는 base 환경과 접근 할 가상환경에 nb_conda를 설치한다.

```powershell
conda install -c anaconda nb_conda
```

참고: <a href="https://anaconda.org/anaconda/nb_conda">https://anaconda.org/anaconda/nb_conda</a>

`nb_conda` 설치 후 아래 그림과 같이 Jupyter 내에서 가상환경 목록을 확인 할 수 있다.

<center><img src="/assets/images/code/2020-10-25-jupyter_notebook_kernel_2.png"></center>