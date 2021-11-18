---
title: "Hugo + github 블로그 만들기 - 4"
date: 2021-11-18T20:18:02+09:00
draft: false
summary: "블로그 글 작성과 배포하기"
---

# 깃허브 블로그 배포해보기

이전 챕터에서 테마를 적용을 완료했으니 이번에는 글을 한번 배포하도록 하겠습니다.

먼저, hugo new site로 만든 디렉터리를 올리기 위해 깃허브 레포지터리를 추가로 생성합시다.

1. https://github.com 에 접속하여 깃허브 레포지터리를 생성합니다.

2. 해당 깃허브 레포지터리와 hugo new site로 만든 디렉터리와 다음과 같은 방법으로 연결합니다.

``` shell
# git remote add origin https://github.com/<yourGithubId>/<yourGithubRepo>
git remote add origin https://github.com/zxcv9203/zxcv9203.github.io.git
```

3. hugo를 이용하여 현재까지 작성한 내용을 정적 사이트로 변환하는 다음 명령을 입력합니다.

``` shell
# hugo -t <theme>
hugo -t github-style
```

4. 3번을 완료하면 public 폴더가 생성될텐데 public 폴더에 git init 후 이전에 만들었던 `<githubId>.github.io`의 레포와 연결합니다.

```shell
cd public
git init
# git remote add origin https://github.com/<yourGithubId>/<yourGithubRepo>
git remote add origin https://github.com/zxcv9203/zxcv9203.github.io.git
```

5. 깃허브에 작성내용을 푸쉬하기 전에 정적 사이트로 변환한 public 디렉터리와 루트 디렉터리를 분리하기 위해 루트 디렉터리에 .gitignore 파일에 

``` shell
cd ..
touch .gitignore
```

```gitignore
# .gitignore file
public/
```

6. 이제 깃허브에 루트 디렉터리와 public 디렉터리의 내용을 푸쉬한다

```
git add .
git commit -m "init" 
git push origin master
cd public
git add .
git commit -m "init"
git push origin master
```

