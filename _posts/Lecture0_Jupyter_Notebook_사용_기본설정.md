---
title: '[Blog] time series- Jupyter_Notebook_사용_기본설정'
categories:
    - Blog

tag:
    - Jupyter Notebook
    - time series

last_modified_at: 2021-07-12T23:00:00+09:00
toc: true
---

# **Python**
- 간결하고 쉬운 컴퓨터와의 소통언어
- 데이터분석과 머신러닝을 위한 수많은 라이브러리를 포함
- 다양한 확장이 용이(ex. R, SPSS, etc.)

> **Anaconda**
> - Python기반의 Open Data Science Platform
> - 추가적인 Library를 연결 및 확장하여 개발/웹/오피스/비즈니스로 활용가능
>> - 어떤 라이브러리를 써야할지 잘 모르겠을때: https://github.com/vinta/awesome-python
>> - 주목받는 파이썬 프로젝트들을 둘러보고 싶을때: https://github.com/trending/python

> **Jupyter Notebook / Jupyter Lab**
> - Interactive 환경을 인간에게 제공하여 컴퓨터(Programming Platform)와 소통을 쉽게함
> - Anaconda 프로그래밍 환경을 그대로 사용
> - 코딩하면서 바로 결과를 확인할 수 있음
> - 문서화(Markdown)와 클라우드(Git, Google Drive 등) 연결/저장 등이 가능
> - 각종 단축키와 튜터리얼 등의 자료가 많음

# **1. 분석환경 세팅(윈도우10기준)**  

## **PIP & Jupyter Notebook & Jupyter Lab 업데이트 및 확장**

**1) 프로그램 다운로드 링크**  
https://www.anaconda.com/download/

**2) Anaconda Prompt 관리자 권한으로 접속**  

**3) Anaconda Prompt: PIP & Jupyter Notebook & Jupyter Lab 업데이트 및 확장 한번에 설치**  
**(하기 내용 복사 후 Anaconda Prompt에 우클릭(붙여넣기))**  
:: Update of PIP  
pip install --upgrade pip  
python -m pip install --user --upgrade pip  
:: Jupyter Nbextensions  
pip install jupyter_contrib_nbextensions  
jupyter contrib nbextension install --user  
:: Jupyter Lab  
pip install jupyterlab  
pip install --upgrade jupyterlab  
:: Jupyter Lab Extensions Package  
pip install nodejs  
conda install --yes nodejs  
conda install -c conda-forge --yes nodejs  
:: Table of Contents  
jupyter labextension install @jupyterlab/toc    
:: Shortcut UI  
jupyter labextension install @jupyterlab/shortcutui  
:: Variable Inspector  
jupyter labextension install @lckr/jupyterlab_variableinspector  
:: Go to Definition of Module  
jupyter labextension install @krassowski/jupyterlab_go_to_definition    
:: Interactive Visualization  
jupyter labextension install @jupyter-widgets/jupyterlab-manager    
jupyter labextension install lineup_widget  
:: Connection to Github  
jupyter labextension install @jupyterlab/github   
:: CPU+RAM Monitor  
pip install nbresuse    
jupyter labextension install jupyterlab-topbar-extension jupyterlab-system-monitor    
:: File Tree Viewer  
jupyter labextension install jupyterlab_filetree  
:: Download Folder as Zip File  
conda install --yes jupyter-archive  
jupyter lab build  
jupyter labextension update --all  
:: End  
    

## **1-3) 선택세팅: Jupyter Lab에 R연결**

**1) 인터넷: R프로그램 다운로드 및 설치** 
- https://cran.r-project.org/bin/windows/base/ 접속  
- "Download" 클릭 및 실행  
**2) 인터넷: Git 설치**  
- https://git-scm.com 접속  
- "Download" 클릭 및 실행  
**3) PC: Anaconda Prompt 접속**  
- 시작 -> "Anaconda" 타이핑  
- "Anaconda Prompt" 우클릭으로 "관리자 권한으로 실행"  
**4) Anaconda Prompt: Jupyter Client & R설치**  
- conda install --yes -c anaconda jupyter_client  
- conda install --yes -c r r-essentials  
**5) Anaconda Prompt: R패키지 설치**
- cd C:\Program Files\R\R-3.6.1\bin(R이 설치된 경로로 이동)
- R.exe   
- install.packages("devtools")  
- devtools::install_github("IRkernel/IRkernel")  
- IRkernel::installspec(user = FALSE) 

# **2. 분석준비 설정(윈도우10기준)**  

## **2-1) Jupyter Notebook & Jupyter Lab 열기 및 내부 설정**

**1) PC: Jupyter Notebook 작업경로 반영**  
- "Jupyter Notebook" 속성 클릭  
- "대상"에서 "%USERPROFILE%/" 삭제후 본인 작업 폴더 반영(ex. D:\)  
- (필요시:기존) jupyter-notebook-script.py (필요시:변경) jupyter-lab-script.py  
- "시작위치"에서 %HOMEPATH% 삭제후 본인 작업 폴더 반영(ex. D:\)  

**2) PC: Jupyter Notebook Nbextensions 기능 추가**  
- Jupyter Notebook 페이지 상단 Nbextensions 클릭  
- "disable configuration for nbextensions without explicit compatibility" 체크 해제  
- 추가 기능 사용
    - Table of Contents
        - The toc2 extension enables to collect all running headers and display them in a floating window, as a sidebar or with a navigation menu. The extension is also draggable, resizable, collapsable, dockable and features automatic numerotation with unique links ids, and an optional toc cell.
    - Autopep8
        - Use kernel-specific code to reformat/prettify the contents of code cells
    - Codefolding
        - This extension enables the CodeMirror feature to allow codefolding in code cells
    - Collapsible Headings
        - Allows notebook to have collapsible sections, separated by headings
    - Hide Input All
        - toggle display of all code cells' inputs
    - Execute Time
        - Display when each cell has been executed and how long it took
    - Variable Inspector
        - The Variable Inspector extension collects all defined variables and display them in a floating window. The extension is also draggable, resizable, collapsable.

**6) PC: Jupyter Lab**  
- http://localhost:8888/lab 타이핑  
- 좌측 상단 폴더를 활용하여 자료 실행 가능 

# **별첨. 이상 무!**

- 위 내용들이 순서대로 진행되야 하며, 제대로 진행하셨다면 문제 발생 확률 낮음  
- 만약 이상이 있다면 모두 삭제 후 처음부터 하나씩 다시 진행하시길 권장  
