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

bin/build 파일 
~~~text
#!/usr/bin/env bash

set -e

rm -rf public
echo "Production..." && hugo --quiet -b http://blog.younjinjeong.io                    -d public/prod
echo "Staging..."    && hugo --quiet -b http://blog-staging.younjinjeong.io                    -d public/staging --buildDrafts
echo "Local..."      && hugo --quiet -b http://localhost                                       -d public/local --buildDrafts $@
~~~ 

bin/new_post 파일 
~~~text
#!/usr/bin/env bash

[ -z $1 ] && echo "Usage: $0 post-name" && exit 1

hugo new post/$1.md -k yaml
~~~

bin/watch
~~~text
#!/usr/bin/env bash

$(dirname $0)/build server
~~~







    