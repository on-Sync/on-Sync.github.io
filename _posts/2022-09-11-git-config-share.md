---
layout: post
current: post
cover:
navigation: True
title: Git 설정하기 (2) - 설정 공유하기
date: 2022-09-11
tags: ["Git"]
class: post-template
subclass: 'post'
author: on-Sync
---

# 설정공유

> 이전 글에서 설정들은 개발자 개인환경에서 적용해야하는 설정입니다.  
> 개인별로 적용하다보면 실수가 있기 마련입니다.  
> 이를 방지하는 방법으로 다음 방법이 있습니다.

## .gitconfig
   
- 설정정보를 공유하기 위해서는 저장소에 `.gitconfig` 로 등록합니다.   
- `git config --local include.path .gitconfig` 명령을 수행하여 같은 환경을 적용할 수 있습니다.   
  - (가능하면 프로그램의 PreRun 에서 git config 를 자동 수행하도록 구성하는 것도 좋습니다.)   
- 글로벌 설정은 `/etc/gitconfig` 파일의 내용으로 공유할 수 있습니다.   
  - 프로젝트별로 다른 설정이 필요할 수도 있기에 상황에 따라 사용합니다.

> 하지만 이 방법은 자동적용이 안되므로 다음 방법을 활용합니다.

## .gitattributes ([설정 상세참고](https://git-scm.com/docs/gitattributes))

- `.gitignore` 와 같이 프로젝트 내부에 git 설정을 저장합니다.
- 별도로 사용할 경우, `'.git/info/attributes'` 에 지정합니다.
- `pattern attr1 attr2 ...` 으로 파일 유형별로 설정할 수 있습니다.

```shell
# 설정 방법 예시
# 1. core.autocrlf=true
*.java text=auto
# 2. core.eol crlf
*.java eol=crlf 
# 3. core.eol lf
*.md eol=lf
# 4. 파일타입 지정
*.java text
*.java binary 
```