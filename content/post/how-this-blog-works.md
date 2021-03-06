---
authors:
- younjin
categories:
- Tip
- Static Web 
- CI/CD
date: 2017-11-27T16:16:22Z
draft: false
short: |
  CI/CD 의 기본 방법을 사용한 본 블로그의 동작 방식에 대해 소개 합니다. 
title: 이 블로그는 어떻게 동작하는가
package used: GitHub, Hugo, TravisCI, Pivotal Web Services, PaperTrail  
---

# 소개 

많은 현대적 시스템들은 서비스 다운 타임을 가지지 않는 경우가 많다. 리소스를 언제든 할당 받을 수 있는 클라우드 시스템의 등장과 함께, 다운타임이나 롤백이 무서워서 
업데이트를 하지 못하는 형태로 서비스를 운영하고자 하는 조직은 별로 없다. 이때 사용 가능한 수많은 도구들의 옵션이 있지만, 오늘은 간단히 애플리케이션의 개발, 빌드, 
테스트, 그리고 배포로 연결되는 흐름을 블로깅 하는데 사용해 본 내용을 소개해 보고자 한다. 


구성하기에 따라서는 유료 서비스를 사용할 수도 있고, 무료 서비스만으로 구성하는것도 가능하다. 

포스팅에서 사용한 도구는 아래와 같다.

- Pivotal Engineering Blog Github : Hugo 라는 블로깅 도구를 위한 기본 템플릿을 가져다가 사용했다. 
- [GitHub](https://github.com) : 기본적으로는 코드 저장소지만 Static Web을 서비스하는 용도로도 사용할 수 있다. 자세한 설명은 (https://pages.github.com) 
- [Hugo](http://gohugo.io) : Static 블로깅 도구로, 마크다운(Markdown) 을 사용해서 포스팅을 작성하거나 테마를 지정하면 static file 로 구성된 웹 페이지를 빌드해 준다. 
hugo server 의 명령어로 실시간으로 페이지가 업데이트 되는 모습을 확인할 수도 있다. 
- TravisCI : 빌드 및 테스트 도구로 사용했으며, Github 의 어카운트로 로그인해서 저장소 리스트를 Sync 한다. 이후에 CI 를 적용할 저장소를 지정하면 된다.   
- [PWS - Pivotal Web Services](https://run.pivotal.io) : 빌드된 애플리케이션을 배포 및 운영하는데 사용한다. https://run.pivotal.io 에서 등록하면 60일 정도 무료로 사용이 가능하다.  
- PaperTrail : 애플리케이션에서 발생하는 로그들을 전달 받아 저장 / 검색 등을 할 수 있도록 해 주는 서비스다.
- Amazon Web Services : 기본적으로는 Route 53 과 같은 DNS 서비스를 사용하기 위해 넣었지만, 필요한 경우에는 CDN 등 다양한 서비스를 함께 사용할 수 있겠다. 

위에서 설명한 대부분의 서비스들은 가입만으로 블로그 정도를 운용하는데 충분한 도구들을 제공한다. 만약 별도의 커스텀 도메인을 사용하지 않는다면 완전히 무료로 구현하는 것도 가능하다. 

본 포스팅의 목적은 언어/프레임워크를 제외하여 CI/CD 본연의 모습을 소개하는데 있다. 소개된 구성을 참조하여 지금 운영하고 있는 서비스와는 무엇이 다른지, 그리고 지난날의 서비스들과는 무엇이 
다른지 한번 생각해 보면 좋겠다. 

물론 Spinnaker 와 같은 도구들을 이미 사용중이라면 "뒤로" 를 누르면 된다.


---

# 준비 

1. Pivotal Engineering Blog 저장소를 클론한다.  

    ~~~text
    $ git clone https://github.com/pivotal/blog 
    ~~~

2. hugo 를 로컬 머신에 준비한다. [여기의 설치정보](https://gohugo.io/getting-started/quick-start/)를 참조해 보자. OSX Homebrew 의 사용자라면 

    ~~~text
    $ brew install hugo
    ~~~ 
    
    를 통해 간단히 설치할 수 있다. 
    
    
3. 위에서 받아온 피보탈의 엔지니어링 블로그가 로컬 머신에서 정상 동작하는지 확인해 보자. 

    ~~~text
    $ cd your/directory/to/blog 
    $ hugo server 
    어쩌고 저쩌고 로그 슝슝슝 
    http://localhost:1313 
    ~~~ 
    
    stdout 이 알려준대로 브라우저로 http://localhost:1313 에 접근해서 페이지가 나오는지 보도록 하자. 잘 나온다면 되었다. 
    
    
--- 

디렉토리 구조를 조금 살펴보면, 몇가지 흥미로운 점을 발견할 수 있다. 

- bin: 블로그를 빌드하고, 새로운 포스트를 작성하고, 정상 동작하는지 확인할 수 있는 간단한 스크립트들이 있다. 나중에 TravisCI 에서 사용한다. 
- content: 새로운 블로그 포스팅을 만드려면 이 하위 디렉토리에 생성된다. 
- themes: 블로그에 사용할 테마가 저장된 디렉토리다. config.yaml 파일을 통해 손쉽게 변경이 가능하다. 또한 현재 기본으로 사용중인 피보탈 엔지니어링 블로그의 레이아웃을 수정할 수 있기도 하다. 
- static: 블로그가 사용하는 css, js, image 등의 static 리소스가 저장된다. 

여기서 bin 디렉토리르 살펴보자. 

bin/build 파일 // TravisCI 나 로컬에서 블로그를 빌드할때 사용한다. 
~~~text
#!/usr/bin/env bash

set -e

rm -rf public
echo "Production..." && hugo --quiet -b http://blog.younjinjeong.io                    -d public/prod
echo "Staging..."    && hugo --quiet -b http://blog-staging.younjinjeong.io                    -d public/staging --buildDrafts
echo "Local..."      && hugo --quiet -b http://localhost                                       -d public/local --buildDrafts $@
~~~ 

bin/new_post 파일 // 새로운 포스팅을 작성할때 사용한다. bin/new_post NEW_POST_NAME 
~~~text
#!/usr/bin/env bash

[ -z $1 ] && echo "Usage: $0 post-name" && exit 1

hugo new post/$1.md -k yaml
~~~

bin/watch  // localhost:1313 에 서버를 구동한다. 
~~~text
#!/usr/bin/env bash

$(dirname $0)/build server
~~~

--- 

# Hugo 

휴고의 설정 파일은 프로젝트 루트의 config.yaml 에 포함되어 있다. 이 파일을 참조해서 각자의 블로그를 설정하도록 하자. 

~~~yaml
languageCode: en-us
Theme: "yjeong-ui"
Title: Younjin Jeong's Blog
taxonomies:
  tag: "tags"
  category: "categories"
  author: "authors"
Params:
  SubTitle: Get busy living, or get busy dying
baseURL: "http://blog.younjinjeong.io"
# googleAnalytics: UA-39702075-1
rssLimit: 25
...
~~~


블로그의 동작을 확인 했다면, 이제 Github 에 신규 repository 를 만들어 블로그를 push 하자. 

~~~text
$ cd blog 
$ git init 
$ git remote add origin https://github.com/your/repo.git 
$ git add . 
$ git commit -m 'initial blog commit' 
$ git push origin master 
~~~

Git 및 Github 의 사용법에 대해서는 별도로 서술하지는 않기로 한다. 


# Travis CI 

Travis-CI 의 경우 처음 페이지에 접근하면 (https://travis-ci.org) GitHub 계정의 권한을 요구하는데 필요하니까 주도록하자. 그러고 나면 한참동안 GitHub 의 코드 저장소들을 스캔한다. 블로그 목적으로 만든 저장소가 스캔되면 설정을 통해 아래의 환경 변수를 설정한다. 

````
|      key     |            value            | 
|--------------|-----------------------------| 
| $GITHUB_TOKEN| YOUR_TOKEN                  | 
| $CF_PASSWORD | YOUR_CLOUD_FOUNDRY_PASSWORD | 
| $CF_USER     | YOUR_CLOUD_FOUNDRY_USESR    |
````

보통 여기에는 다른 시스템에 대한 접근이 필요하지만 관련된 credential 을 코드 안에 넣을 수 없으므로 이곳에서 시스템 환경 변수로 설정해서 사용한다. 또는 각자의 목적에 따라 필요에 맞게 사용하면 되겠다. 

![Travis CI](/images/how-this-blog-works/TravisCI-env.png "Travis CI")

Travis CI 의 동작 방식은, 지정된 repository 에 .travis.yml 파일을 추가해 주면 동작한다. 아래는 현재 사용중인 설정이다. 기본적으로 hugo 바이너리를 적절한 위치에 준비하고, git 에 업로드된 블로그 소스를 바탕으로 static 파일들을 production/statging/local 용도로 public 디렉토리에 빌드하고 이를 deploy 커맨드를 통해 배포하는 형태다.  

~~~yaml
# Install the apt prerequisites
addons:
  apt:
    packages:
      # - python-pygments   # Syntax highlighting is already provided

# Clean and don't fail
install:
  - rm -rf public || exit 0

# Build the website
script:
  - tar xvzf bin/hugo_0.31_Linux-64bit.tar.gz -C bin/
  - chmod +x bin/hugo
  - export PATH=$PATH:bin/
  - bin/build

# Deploy to GitHub pages, 이 부분은 블로그를 깃헙의 페이지를 이용해서 구성하고자 하는 경우 사용할 수 있다. 
deploy:
  - provider: pages
    repo: younjinjeong/younjinjeong.github.io
    skip_cleanup: true
    local_dir: public/tech/
    github_token: $GITHUB_TOKEN # Set in travis-ci.org dashboard
    on:
      branch: master

# Pivotal Web Service 에 배포하는 부분, 자신의 target 에 맞도록 수정한다. 
  - provider: cloudfoundry
    username: $CF_USER
    password: $CF_PASSWORD
    api: https://api.run.pivotal.io
    organization: yjeong-org
    space: staging
    local_dir: public/staging
~~~

# Cloud Foundry 

클라우드 파운드리에 배포는 위의 Travis CI 를 통해 진행되지만, 이때 프로젝트 루트의 manifest.yaml 파일을 참조한다. 파일은 아래와 같다. 

~~~yaml 
---
applications:
#- name: pushpop
#  path: ./pushpop
#  memory: 64M
#  disk: 200M
#  command: bundle exec pushpop jobs:run
#  no-route: true
#  health-check-type: none
#  services:
    # - keen.io
    # - pushpop-sendgrid-account
- name: younjinjeong-blog
  path: ./public/prod
  memory: 64M
  disk: 200M
  buildpack: https://github.com/cloudfoundry/staticfile-buildpack.git
  services: 
    - blog-autoscaler   # cf create-service app-autoscaler blog-autoscaler, 이후 바인드 
    - blog-younjinjeong-log  # PaperTrail 연결을 위한 Custom 서비스, 이후 설명 
- name: younjinjeong-staging
  path: ./public/staging
  memory: 64M
  disk: 200M
  buildpack: https://github.com/cloudfoundry/staticfile-buildpack.git

- name: younjinjeong-cdn
  path: ./public/cdn
  memory: 64M
  disk: 200M
  buildpack: https://github.com/cloudfoundry/staticfile-buildpack.git
~~~

이를 그대로 사용하면 blog-autoscaler 가 없다는 메세지를 받게 될 것이다. 이를 위해서 로그인한 PWS(PCF)에서 cf create-service 를 수행해 줄 필요가 있다. 

~~~text
$ cf login -a https://api.run.pivotal.io 
$ cf create-service app-autoscaler blog-autoscaler  
$ cf bind-service younjinjeong-blog blog-autoscaler  # younjinjeong-blog 는 본인이 사용할 앱이름을 쓰도록 한다 
~~~



# PaperTrail 

페이퍼 트레일은 원격 로깅을 위해 사용한다. 비슷한 방법으로 구글이나 스플렁크와 같은 서비스에 애플리케이션 로그를 전달하는 것이 가능하다. 페이퍼 트레일(https://papertrailapp.com/) 페이지에 가서 사인업하고 새로운 애플리케이션을 만들면 엔드포인트를 하나 할당 받을 수 있다. 

그럼 아래와 같은 커맨드를 통해 cf 의 커스텀 서비스로 등록이 가능하다. 

~~~text
$ cf cups blog-younjinjeong-log -l syslog-tls://xxxx.papertrailapp.com:xxxxx
$ cf bind-service blog-younjinjeong blog-younjinjeong-log # Blog 및 서비스 이름은 원하는것을 사용하도록 한다. 
~~~

![Travis CI](/images/how-this-blog-works/PaperTrail.png "PaperTrail")


여기까지 정상적으로 설정 되었다면 github 에 커밋과 푸시가 발생할 때마다 빌드와 배포의 동작이 반복된다. 테스트 해 보도록 하자. 


# 결론 

본 구성의 핵심은 어떤 도구를 사용하더라도, 코드나 문서를 작성하고 이를 빌드하고 배포하는 과정에 있어 최대한 자동화를 구성하는 것이다. 빌드에 문제가 있거나, 만약 추가할 수 있는 테스트에 문제가 발생한다면 (shell exit code가 0이 아니라면) 배포는 진행되지 않을것이다. 이는 단순히 블로그를 가지고 구현한 것이지만, 구성을 참조한다면 코드 딜리버리 파이프라인에 확장해서 사용할 수도 있겠다. 
