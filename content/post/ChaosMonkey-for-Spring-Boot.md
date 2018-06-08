---
authors:
- yjeong

categories:
- Chaos Toolkit
- Spring Boot 
- Spring driver 
- Chaos Monkey
- Micro Services

date: 2018-06-07T17:16:22Z
draft: false
short: |
  새로운 스프링 드라이버와 카오스 툴킷을 사용한 애플리케이션 레벨의 카오스 엔지니어링 소개 
title: 스프링 부트와 카오스 몽키 
image: /images/ChaosMonkey-for-Spring-Boot/SpringDriver.png

---

# 오리지날 블로그 포스트 

여기에 소개된 내용의 원문은 [Russ Miles (@russmiles)](https://twitter.com/russmiles)에 의해 작성 되었으며, 원문 링크는 아래와 같다. 

_translated from [Chaos Toolkit LOVES Chaos Monkey for Spring Boot](https://medium.com/chaos-toolkit/chaos-toolkit-loves-chaos-monkey-for-spring-boot-548352985c8f)..._ 

[원문 링크](https://medium.com/chaos-toolkit/chaos-toolkit-loves-chaos-monkey-for-spring-boot-548352985c8f)


- 오역 및 잘못된 내용은 언제든 (younjin.jeong@gmail.com)으로 보내주세얌. 
- 가장 자주 등장하는 assault는 공격으로, watcher는 감시자로 번역 합니다. 

# 시작에 앞서, 카오스 엔지니어링 소개 및 본 포스팅의 취지 

시작전 겁나 긴 역주: 

카오스 엔지니어링은 일반적으로 서비스를 테스트의 목적으로 일부러 망가트려 보고, 이에 대한 시스템의 반응을 바탕으로 더 견고한 서비스를 만들기 위한 엔지니어링을 의미한다. 

이 부분에서 가장 유명하다고 할 수 있는 넷플릭스는 자사가 개발한 다양한 카오스 엔지니어링 도구를 오픈 소스로 배포하고 있는데, 그들 중 가장 유명한 것이 바로 원숭이 군단(Simian Army)로 불리는 것들이다. 그 가운데에서도 잘 알려진 것은 바로 카오스 몽키로, 실 서비스에서 동작하는 인스턴스의 종류와 관계 없이 마구 서버를 죽이며 돌아다니는 도구다. 당연히 데이터베이스나 카프카 클러스터의 멤버, 웹 서비스나 스트리밍 서비스 멤버의 서버들을 마구 죽인다. 

넷플릭스는 실 서비스에 배포를 위한 도구들에 이 테스트를 적용할 뿐만 아니라, 실제로 동작하는 서비스에도 적용하는 것으로 잘 알려져 있다. 카오스 몽키 뿐만 아니라 전체 가용 존(Availiablity Zone)을 죽이거나, 또는 하나의 AWS 리전(Region) 전체를 죽이는 테스트 역시 수행하는 것으로 알려져 있다. 

이 포스팅에서는 스프링 부트용 카오스 몽키 도구를 소개하고, 간략하게 사용법을 알아본다. 기본적으로 애플리케이션 수준에서 동작하므로, 스프링 부트 애플리케이션의 설정을 사용하도록 구성 되었다. 

언뜻 보기에 스프링 부트를 그냥 죽이거나 요청에 대한 응답을 모사하기 위한 간단한 도구로 보일 수 있지만, 테스트 자동화 도구등을 통해 서비스에 함께 구현된 서킷 브레이커나 리트라이(Retry)등의 패턴을 통해 의존 관계에 있는 다른 서비스에 문제가 생겼을때 어떻게 되는지에 대한 실험과 검증의 자동화 방법으로 사용할 수 있는 도구라는 점이 중요하다. 

카오스 몽키 도구는 애플리케이션이 동작하는 서비스를 무작위로 종료하는 것으로 잘 알려져 있지만, 이는 애플리케이션 수준의 테스트라고 보기는 힘들기 때문에 이와 같은 도구의 사용을 통해 실제 서비스에 문제가 되더라도 장애를 극복할 수 있는 애플리케이션 테스트에 사용할 수 있다는 점에서 카오스 엔지니어링 부분에 상당히 중요한 도구라고 할 수 있겠다. 

필요한 코드는 모두 공개 되었으므로 적용 방법을 수월하게 살펴볼 수 있을 것이다. 


# 스프링 툴킷과 스프링 부트용 카오스 몽키 

[카오스 툴킷](https://chaostoolkit.org/)의 첫번째 릴리즈에서는 스프링 부트에서 카오스 몽키를 애플리케이션 레벨에서 지원하기 위해 [chaostoolkit-spring driver](https://chaostoolkit.org/extensions/spring)를 제공한다. 

>"[chaostoolkit-spring 인큐베이터 프로젝트](https://github.com/chaostoolkit-incubator/chaostoolkit-spring)는 오픈 소스이며, 자유롭게 사용 가능하다."

{{< responsive-figure src="/images/ChaosMonkey-for-Spring-Boot/SpringDriver.png" class="center" >}}

이 첫 릴리즈는 스프링 부트 애플리케이션과 서비스에 다양한 카오스 엔지니어링을 사용할 수 있도록 [스프링 부트용 카오스 몽키](https://github.com/chaostoolkit-incubator/chaostoolkit-spring) 지원이 아래의 내용과 함께 [codecentric](https://github.com/codecentric)에 의해 포함되었다. 
 

- 특정 서비스의 런타임에서 **카오스 몽키 기능을 켜거나 끌 수** 있다. (카오스 엔지니어링 실험을 하는 동안만 사용하고자 할때 매우 유용하다.)
- 특정 서비스의 런터임에서 **카오스 공격(assults) 설정과 활성화를 지원**한다. 
- **카오스 몽키의 와처(watcher)와 어썰트(assaults) 설정을 기록하고 감사**할 수 있다. 이런 정보의 수집은 향후 분석을 위해 매우 유용하다. 


이 같은 동작은 카오스 툴킷 정의에 포함되어 있다. 몇가지 예제를 살펴보자. 

> 여기에 소개된 예제들은 모두 [깃헙 링크](https://github.com/chaosiq/demos/tree/master/spring-boot-chaos-monkey)에서 찾아볼 수 있다. 


## 실험 예제 1: "의존 관계에 있는 서비스가 죽으면 우리 서비스는 어떻게 되지?" 

스프링 부트용 카오스 몽키에서 제공하는 가장 기본적인 기능은, 스프링 부트 애플리케이션이 스스로를 죽여 봄으로서 의존 관계에 있는 다른 서비스가 어떻게 대응하는지 실험해 보는 것이다. 이 기능은 아래의 가설을 실험하기 위한 것이다. 

> "내 서비스가 의존하고 있는 다른 서비스가 의도치 않게 죽으면 어떻게 될 것인가?" 

이 예제에서는 consumer와 producer, 2개의 서비스를 준비 한다. consumer는 별도의 장애에 대한 고려 없이 producer를 직접 참조하는 구조로 되어 있다. 

예제에 정의된 실험을 스프링 부트용 카오스 몽키를 사용 하면 chaostoolkit-spring을 사용하면 이후 보안 취약점이 될 수 있는 점을 잊지 말자. (역주: 따라서 테스트 이외의 프로덕션 배포에도 사용하는 것은 주의 하도록 하자)

[Github link](https://gist.github.com/russmiles/7f4a99110fa9cb6d90025429845bc463#file-kill-dependency-experiment-json)

~~~json 
{
    "version": "1.0.0",
    "title": "Setting and triggering service dependency death",
    "description": "Uses the Spring Chaos Monkey to manipulate a service",
    "tags": [
        "service",
        "spring"
    ],
    "steady-state-hypothesis": {
        "probes": [
            {
                "name": "consumer-service-must-still-respond",
                "provider": {
                    "type": "http",
                    "url": "http://localhost:8080/invokeConsumedService"
                },
                "tolerance": 200,
                "type": "probe"
            }
        ],
        "title": "System is healthy"
    },
    "method": [
        {
            "name": "enable_chaosmonkey",
            "provider": {
                "arguments": {
                    "base_url": "http://localhost:8090/actuator"
                },
                "func": "enable_chaosmonkey",
                "module": "chaosspring.actions",
                "type": "python"
            },
            "type": "action"
        },
        {
            "name": "configure_assaults",
            "provider": {
                "arguments": {
                    "base_url": "http://localhost:8090/actuator",
                    "assaults_configuration": {
                        "level": 1,
                        "latencyRangeStart": 2000,
                        "latencyRangeEnd": 5000,
                        "latencyActive": false,
                        "exceptionsActive": false,
                        "killApplicationActive": true,
                        "restartApplicationActive": false
                    }
                },
                "func": "change_assaults_configuration",
                "module": "chaosspring.actions",
                "type": "python"
            },
            "type": "action"
        },
        {
            "name": "trigger-kill",
            "provider": {
                "type": "http",
                "url": "http://localhost:8080/"
            },
            "type": "probe"
        }
    ],
    "rollbacks": []
}
~~~ 

이 실험에서 첫째로 확인해야 하는 것은 provider 서비스에서 enable_chaosmonkey를 사용해서 스프링 부트용 카오스 몽키를 활성화 했는가의 여부다. (라인 25 참고)

두번째로는 서비스 호출 또는 요청이 발생 할때마다 killApplicationActive 공격 트리거가 동작하도록 활성화 되어 있는지 확인하자. (라인 37 참고)

마지막으로 정상 상태 가정(steady-state-hypothesis)을 정의하고 (라인 9 참고), killApplicationActive 공격(라인 58)을 트리거 하자. 


공격은 스프링 부트 애플리케이션 설정의 감시자(watcher)를 통해 트리거 된다. 이 설정은 스프링 부트용 카오스 몽키에서 제공되며, 런타임에서 변경할 수 없다. 아래의 기본 설정의 내용은 provider 애플리케이션이 (@Service 사용으로 인해) 유입되는 모든 요청에 watcher를 동작하도록 한다. (라인 13 참고)

[Github Link to Spring Boot application properties](https://gist.github.com/russmiles/df97c46677e5b6c7eb428cd8af6f06d3#file-application-properties)

~~~java 
spring.application.name=ChaosDemo
server.port=8090

spring.profiles.active=chaos-monkey
chaos.monkey.enabled=false

management.endpoint.chaosmonkey.enabled=true

management.endpoints.web.exposure.include=*

chaos.monkey.assaults.latencyActive=false
chaos.monkey.assaults.killApplicationActive=false
chaos.monkey.watcher.restController=true

chaos.monkey.assaults.level=1
~~~

이를 통해 provider 서비스에 문제가 발생했을때 consumer 서비스가 어떻게 장애에 대응할 수 있는지 실험해 볼 수 있다. 


## 실험 예제 2. "의존 관계에 있는 서비스의 응답이 느려지면 어떻게 하지?" 

스프링 부트용 카오스 몽키에는 지연시간 설정 기능이 포함되어 있으며, 아래의 가정을 실험하기 위해 제공된다. 

>"provider 서비스의 응답이 갑자기 느려진 상황에서 consumer 서비스는 어떻게 대응하는가?" 

[Github link to Latecy configuration](https://gist.github.com/russmiles/c07710726370584053b31bd749a2ddd2#file-latency-dependency-experiment-json)

~~~json 
{
    "version": "1.0.0",
    "title": "Exploring assumptions if a dependency starts responding slowly",
    "description": "Uses the Spring Chaos Monkey to manipulate a service",
    "tags": [
        "service",
        "spring"
    ],
    "steady-state-hypothesis": {
        "probes": [
            {
                "name": "consumer-service-must-still-respond",
                "provider": {
                    "type": "http",
                    "url": "http://localhost:8080/invokeConsumedService"
                },
                "tolerance": 200,
                "type": "probe"
            }
        ],
        "title": "System is healthy"
    },
    "method": [
        {
            "name": "enable_chaosmonkey",
            "provider": {
                "arguments": {
                    "base_url": "http://localhost:8090/actuator"
                },
                "func": "enable_chaosmonkey",
                "module": "chaosspring.actions",
                "type": "python"
            },
            "type": "action"
        },
        {
            "name": "configure_assaults",
            "provider": {
                "arguments": {
                    "base_url": "http://localhost:8090/actuator",
                    "assaults_configuration": {
                        "level": 1,
                        "latencyRangeStart": 10000,
                        "latencyRangeEnd": 10000,
                        "latencyActive": true,
                        "exceptionsActive": false,
                        "killApplicationActive": false,
                        "restartApplicationActive": false
                    }
                },
                "func": "change_assaults_configuration",
                "module": "chaosspring.actions",
                "type": "python"
            },
            "type": "action"
        }
    ],
    "rollbacks": []
}
~~~ 

지연 시간 설정을 위해 이전 설정과 다른 부분은 라인 43-45에 있다. 이 부분에서 카오스 도구를 통해 서비스와 요청에 대한 지연 시간 파라 메터를 설정 했다. 이 설정을 통해 consumer 서비스는 아주 느리게 응답하는 provider 서비스에 대해 어떻게 반응하는지를 실험해 볼 수 있다. 


이 두가지의 간단한 예제는 카오스 툴킷을 사용해서 애플리케이션 레벨에서 발생할 수 있는 취약점을 실험할 수 있다. 추가로 소개되는 [데모 코드](https://github.com/chaosiq/demos/tree/master/spring-boot-chaos-monkey)를 통해 어떤 기능을 더 사용할 수 있는지 살펴보자. 우리 개발팀은 여러분의 피드백을 언제나 환영한다. 


# 오픈 소스와 확장 

카오스 툴킷을 무료 오픈 소스로 공개하는 목적은 애플리케이션 수준에서 카오스 엔지니어링을 **_더 쉽고 빠르게_** 사용할 수 있도록 하는 것이며, 가능한 경우 새로운 오픈 소스 또는 상용 프로젝트로 손쉽게 발전을 위해서이다. 

[카오스 툴킷](https://chaostoolkit.org/) 커뮤니티는 스프링 부트용 카오스 몽키 프로젝트의 발전과 더 많은 사용자들이 모든 기능을 사용할 수 있도록 노력할 것이다. 


# 카오스 툴킷 프로젝트에 참여하기 

[카오스 툴킷](https://chaostoolkit.org/)은 커뮤니티가 주도하는 오픈 소스 프로젝트이며, 모든 사용자가 카오스 엔지니어링을 통한 경험을 자동화하는 것을 목표로 한다. 특정 벤더에 종속적이지 않은 자유로운 프로젝트이며, 카오스 엔지니어링에 대해 배울 수 있는 좋은 프로젝트인 동시에, 여러분의 경험을 위한 기반으로 사용할 수 있으며 여러분의 카오스 엔지니어링 경험과 아이디어를 가져올 수도 있다. 

언제든 [이슈 및 새로운 기능을 제기](https://github.com/chaostoolkit)하거나, 코드를 작성하거나, [우리의 슬랙](https://join.chaostoolkit.org/) 채널에 가입을 통해 이 프로젝트에 기여할 수 있다. 

