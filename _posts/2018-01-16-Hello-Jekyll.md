---
layout: post
title: "How to make jekyll blog in OSX"
date: 2018-01-16 01:47:00 +0900
description: 지킬 블로그를 만들고나서 처음 쓰는 시험용 포스트
img: # None
---

처음으로 Jekyll에 글을 써본다. 블로그는 Flexible이라는 테마를 임포트해와서 구축해보았다.

## Install Jekyll in local

설치 방법은 아주 간단한데, OSX에는 기본적으로 ruby와 gem이 설치되어있다.

```sh
sudo gem install jekyll
```

그러므로, 위의 명령어를 수행해서 간단하게 jekyll을 설치할 수 있을 것이다.

필자의 경우, OSX High Sierra를 사용하고 있는데, High Sierra버전 들어서 그런 것인지 몰라도, Ruby의 패키지 관리자인 Gem이 Install을 수행할 때 stdio.h 라이브러리를 찾지 못한다는 오류가 떠서 고생하였다.

이 경우,

```sh
xcode-select --install
```

명령어를 실행하고나니까 정상적으로 설치할 수 있었다

## Create new jekyll blog

```sh.
jekyll new my-awesome-site
```

위와 같은 명령어를 통해서 Jekyll 프로젝트 폴더를 생성한 후에, `_posts` 폴더에 쓸 포스트를 Markdown 형식으로 작성해주면 지킬이 알아서 작성해준다.

## Deploying in github page

깃허브 페이지에 해당하는 레포지터리에 푸시 이벤트가 발생할 경우, 깃허브 서버에서 자동으로 Jekyll을 수행(?)해주기 때문에, 잘 작동한다.

이것은 Jekyll만의 특별한 트릭이 숨겨져있는 것이라기 보다는, Github에서 Jekyll을 공식적으로 지원해주기 때문이라고 한다.