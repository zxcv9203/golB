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

``` shell
git add .
git commit -m "init" 
git push origin master
cd public
git add .
git commit -m "init"
git push origin master
```

7. 블로그에 접속 후 일정 시간이 지나면 배포가되는 것을 확인할 수 있습니다.

# 블로그에 글 포스팅해보기

이제 깃허브 블로그를 만들었으니 블로그에 글을 포스팅해봅시다.

기본적으로 마크다운 문법을 사용하여 블로그글을 작성하게 됩니다.

github-style 테마 기준으로 포스팅하는 법을 작성하겠습니다.

## 깃허브 테마 Readme 만들기

깃허브 테마 같은 경우 깃허브 Readme.md를 만들면 실제 깃허브처럼 꾸밀 수 있기 때문에 Readme 먼저 작성해보겠습니다.

1. 다음 명령어로 readme.md를 생성합니다.

``` shell
hugo new readme.md
```

2. content 디렉터리에 존재하는 readme.md를 원하는 대로 수정합니다.

3. 다음 명령어로 로컬에서 원하는데로 만들어 졌는지 확인합니다. (명령어 입력후 localhost:1313으로 접속)

``` shell
# hugo server -t <theme>
hugo server -t github-style
```

4. localhost:1313으로 접속해서 확인해보면 잘 나오는 것을 확인할 수 있습니다.

## 배포 자동화시키기 

현재 같은 경우 루트 디렉터리를 저장하는 레포지터리와 실제 정적 페이지가 저장되어있는 public 디렉터리로 이루어진 레포지터리 두개가 존재하며 변경사항이 있을때마다 hugo 명령으로 계속 변환을 해주어야 합니다.

이렇게 명령들을 여러차례 입력해야 하기 때문에 이 과정을 쉘 하나로 자동화시킬 수 있도록 만들어봅시다.

1. deploy.sh 이라는 이름의 쉘 파일 하나를 만듭니다.

``` shell
touch deploy.sh
```

2. 다음과 같이 작성합니다.

``` bash
#!/bin/bash

echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
# hugo -t <your theme>
hugo -t github-style

# Go To Public folder, sub module commit
cd public
# Add changes to git.
git add .

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master

# Come Back up to the Project Root
cd ..


# blog 저장소 Commit & Push
git add .

msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

git push origin master
```

3. deploy.sh을 실행시키면 배포가 자동으로 이루어집니다.

``` shell
bash deploy.sh
# or
chmod deploy.sh 744 deploy.sh
./deploy.sh
```

## 블로그 글 작성하기

이제 배포 자동화도 했고 메인화면도 꾸몄으니 블로그글을 작성해봅시다.

github-style 기준입니다.

1. 블로그 디렉터리의 루트에 있는 `posts` 폴더를 `post` 폴더로 이름을 변경합니다.

```shell
mv posts post
```

2. hugo 명령을 통해 블로그에 포스팅할 페이지를 생성합니다.

```shell
#hugo new post/<파일이름.md>
hugo new post/first.md
```

3. `post/<파일이름>.md`를 열고 글을 작성합니다. github-style 테마의 경우 다음과 같이 draft의 값을 변경하고 summary를 추가해야합니다. (하지 않을 경우 글을 볼 수 없음)

```markdown
title: "제목"
date: 2021-11-17T13:34:24+09:00
draft: false
summary: "설명"
```


4. 작성을 완료하면 블로그에 올리기 전에 로컬에서 확인하기 위해 다음 명령을 사용합니다.

```shell
#hugo server -t <theme>
hugo server -t github-style
```

5. localhost:1313에 들어가서 확인했을 때 문제가 없다면 이전에 만든 배포 쉘을 이용해 블로그 글을 배포하고 잠시 기다린 후 글이 올라왔는지 확인합니다.

```shell
bash deploy.sh
# or
./deploy.sh
```

6. 블로그 글이 잘 추가된 것을 볼 수 있습니다.

# 🎉 깃허브 블로그 글을 배포 완료했습니다 🎉

이제 깃허브 블로그를 사용하여 글을 작성하면 됩니다.