---
layout: post
current: post
cover: assets/images/post_cover/jekyll-thumbnail.jpg
navigation: True
title: GitHub Pages 블로그 만들기 (1)
date: 2022-08-15
tags: ["Blog", "GitHub", "Daily"]
class: post-template
subclass: 'post'
author: on-Sync
---

# GitHub Pages 란 ?

GitHub Pages 는 Jekyll 과 같은 웹빌더 환경을 제공하는 서비스입니다.

## GitHub Pages 사용방법

- 이 서비스는 기본적으로 `{username}.github.io` 형식의 이름을 갖는 저장소를 생성합니다.
- 저장소 생성 후에는 설정에서 Pages 브랜치를 지정합니다. 이때, GitHub 은 자동으로 업로드된 소스에서 웹빌더를 파악하여 정적 웹파일을 생성 및 제공합니다.
- 기본 URL 은 저장소의 이름인 `{username}.github.io` 로 노출되며, 추가 설정으로 변경할 수 있습니다.

## Jekyll 사용하기

### GitHub Pages 제공버전

Jekyll 은 Ruby 기반의 웹빌더입니다.
GitHub Pages 에서는 Ruby 는 `2.7.4`, Jekyll 은 `3.9.2` 버전으로 제공하고 있습니다.
참고로 현재 최신 Ruby 는 `3.x.x`, Jekyll 은 `4.x.x` 이상 버전이므로, 의존성에 맞는 버전을 사용해야합니다.
   
> 상세항목은 [GitHub Page`s Dependency versions](https://pages.github.com/versions/) 를 참고하시기 바랍니다.

### Jekyll 작업환경 만들기

GitHub Pages 의 단점은 Commit 시점 기준으로 정적파일을 재빌드하고, 오류사항을 제공하지 않습니다.
그렇기에 개발자는 로컬에서 정상여부를 확인한 후에 저장소에 푸시하는 것이 좋습니다.
   
Ruby 와 Jekyll 를 위에 명시된 의존성에 맞게 직접 설치해도 되지만,
여기서는 Docker 를 사용하여 간편하게 구성해보겠습니다.
   
우선 기본 베이스는 Ruby 이미지를 사용합니다.
물론 Jekyll 이미지도 DockerHub 에 존재하지만,
`3.9.2` 버전이 없어서 디자인되어 공유되는 Jekyll 스킨을 사용할 때
피곤해지기에 저는 Ruby 이미지를 베이스로 사용하는 것을 추천합니다.

우리가 사용할 베이스 버전은 `ruby:2.7-alpine3.16` 입니다.
Ruby 에서는 언어 특성상 고가용성보단 저사양 효율성에 알맞기에 `50MB`의 가벼운 alpine os 로 이미지를 제공합니다.

alpine 에서는 OS 패키지 매니저로 apk 를 사용합니다.
기본적인 개발환경은 apk 를 사용하여 설치합니다.

```shell
apk update
apk add --no-cache build-base gcc cmake git curl
```

이 후 Ruby 패키지 매니저인 gem 을 통해서 Jekyll 및 Bundler 를 설치해줍니다.
버전은 `3.9.2` 를 지정하는 것이 심신건강에 좋습니다.

```shell
gem update bundler \
    && gem install bundler \
    && gem install jekyll -v 3.9.2
```

기본적인 환경이 완성되었다면, Jekyll 을 사용하여 새로운 웹 프로젝트를 만들거나 오픈소스로 디자인된 스킨들을 활용합니다.
저는 미적감각이 없으므로 [`Jasper2`](http://jekyllthemes.org/themes/jasper2/) 를 사용하겠습니다.
링크에서 Hompage 를 누르면 GitHub 저장소로 이동하게 됩니다.
다음과 같이 소스를 로컬에 Clone 해줍니다.

```shell
git clone https://github.com/jekyllt/jasper2.git
```

설치된 웹프로젝트에는 기본적으로 Gemfile 에 몇가지 의존성 설정이 되어있습니다.
없어도 기본적인 의존성을 설치해야 하기에 bundler 로 Gem (i.e. `ruby 의 모듈단위`) 들을 다운로드받습니다.

```shell
bundler install
```

설치를 생략하고 Jekyll 을 실행해도 좋지만, 종종 `Required` 오류를 만날 수 있습니다. (e.g. webrick)
이때는 `bundler add {설치요구대상}` 로 해당 요구항목을 Gemfile 의존성에 추가하여 실행합니다.
   
의존성 설치까지 완료되었다면 다음 명령어로 Jekyll 을 실행해주면 됩니다.

```shell
bundler exec jekyll serve -v 3.9.2
```

여러 버전의 jekyll 이 설치되어있다면 버전을 명시해주면 됩니다.
참고로 `jekyll serve` 를 바로 사용한다면 `PATH` 명시된 jekyll 버전을 사용하기에 의존성 문제를 겪을 수 있습니다.
   
만약 Docker 로 실행할 경우, 기본 IP 가 `127.0.0.1` 로 지정되니,
Host 에서 접근가능하도록 `--host 0.0.0.0` 을 추가합니다.
   
추가로 디버깅에 유용한 옵션으로 `--livereload` 를 명시하면 html, css 같은 정적인 파일이 수정되었을 때,
Jekyll 재 실행 없이, 웹화면을 변경시킵니다. 이 옵션이 정상적으로 작동하지 않는다면
파일변경 이벤트를 캐치하지 못하여 Regenerate 를 하지 않았거나
이벤트 연결포트(`default: 35729`) 가 닫혀있을 수 있습니다.
후자는 포트를 열어주면 되고, 전자의 경우 권한이나 여러문제가 있겠지만
기본적으로 `--force-polling` 옵션을 추가해주면 해결됩니다.
   
지금까지의 모든 작업내용을 Docker 로 옮기면 다음과 같습니다.

#### Dockerfile

```shell
FROM ruby:2.7-alpine3.16

LABEL name="gitpages-jekyll"
LABEL email="putstack@gmail.com"
LABEL version="1.0"

# update package manager & install default program
RUN apk update
RUN apk add --no-cache build-base gcc cmake git curl

# install bundler
RUN gem update bundler \
    && gem install bundler \
    # git-pages latest support version (newest is upper 4.2.x, but not supported)
    && gem install jekyll -v 3.9.2

# setting entrypoint
COPY entrypoint.sh /usr/local/bin/entrypoint
WORKDIR /srv/jasper2
ENTRYPOINT ["entrypoint"]
```
#### entrypoint.sh

```shell
#!/bin/sh

bundle install

bundle exec "$@"
```

여기까지 완료했다면 Jekyll 의 Docker 기반 로컬개발환경은 준비완료되었습니다.
관련 소스는 [putstack/gitpages-jekyll 이미지의 생성파일](https://github.com/MyBestPractice/dockerfile-factory/tree/master/gitpages-jekyll)
과 [putstack/gitpages-jekyll 이미지의 컨테이너 실행시 환경설정](https://github.com/MyBestPractice/dockerfile-factory/blob/master/.idea/runConfigurations/gitpages_jekyll_Dockerfile.xml)
에 올려두었습니다.

다음에는 한글폰트를 지정하여 이쁜 글씨로 __포스팅 하는 방법__ 에 대해 얘기해보겠습니다.

감사합니다.