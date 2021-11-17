---
title: "Hugo + github 블로그 만들기 - 3"
date: 2021-11-17T13:34:24+09:00
draft: false
summary: "Github 블로그 만들기와 Hugo를 이용한 정적 웹페이지 생성"
---

# Github 블로그 생성

먼저 Github 블로그 생성을 위해 repository 먼저 생성해봅시다.

1. https://github.com 에 접속하여 로그인합니다. (깃허브 계정이 없다면 다음 빨간 박스를 선택한 후 가입해야 합니다.)

	![github_main](/images/hugo3-1.png)

2. 깃허브 블로그로 사용할 레포지터리를 생성합니다. 

	![github_repo_create](/images/hugo3-2.png)

3. 레포지터리 이름을 다음과 같은 형태로 만듭니다. `<github id>.github.io`

	![github_page_create](/images/hugo3-3.png)

# Hugo를 이용한 정적 사이트 생성하기

웹 개발에 대한 지식이 없기때문에 정적 사이트 생성기를 통해 깃허브 블로그를 사용하기로 했고 이번에는 Hugo(이하 휴고)를 사용해서 블로그를 만들려고한다.

먼저 휴고를 사용하기 위해서 휴고를 설치해야하는데 brew를 사용하면 간단하게 설치할 수 있습니다.

저의 경우 WSL2를 이용해 우분투를 사용해서 linux homebrew를 설치하여 해결하였습니다.

먼저 linux homebrew 먼저 설치해봅시다.