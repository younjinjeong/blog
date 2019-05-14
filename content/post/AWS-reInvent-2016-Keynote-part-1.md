---
authors:
- younjin
categories:
- News
- AWS 
- reInvent
date: 2016-11-18T06:34:12-04:00
short: |
  2016년 AWS re:Invent 키노트 첫날 소개된 서비스들에 대한 간략한 정리   
title: AWS re:Invent 2016 Keynote 1, New services 
---

(younjin.jeong@gmail.com, 정윤진) 

오랜만에 아마존 웹 서비스 관련 내용. 아마존 웹 서비스는 매년 10월-12월 사이의 겨울에 글로벌 규모의 행사를 진행한다. 이 행사는 re:Invent 라고 불리며 매년 참가자가 늘어나고 있다. 
올해 2016년에는 3만 5천명 규모의 행사로 라스베가스의 베네시안에서 진행된다. 여러해 동안 아마존 웹 서비스와 이렇게 저렇게 인연을 쌓아왔지만 re:Invent 에 참석하는 것은 처음이다. 어마어마한 사람들, 해를 거듭할 수록 쏟아지는 새로운 서비스들, 그리고 이 서비스들을 사용한 다양한 경험의 공유와 더 잘 사용하기 위한 팁 등이 발표된다. 

{{< responsive-figure  src="http://cfile22.uf.tistory.com/image/227B1E3A583F326229195B" >}}

2016년 올해도 다양한 서비스들이 발표 되었다. 전반적인 서비스 개선의 흐름을 보자면 사용성의 개선을 통한 신규 사용자의 진입 장벽을 낮추고, 데이터 분석과 관련된 신규 서비스들이 발표 되었으며 특히 아마존 에코를 통해 축적된 음성 인식 기술을 보편적으로 사용할 수 있도록 하는 서비스들, 이미지 분석, 그리고 대량의 데이터 이전 등 첫날의 발표는 다소 데이터에 집중하는 모습이다. 

아마존 웹 서비스는 탄탄한 기반의 EC2와 S3를 바탕으로 지속적으로 서비스 포트폴리오를 확장해 왔는데, 이번에도 역시 아마존의 서비스를 지원하기 위해 필요했던 다양한 기술, 그리고 아마존의 경험이 녹아있는 기술들이 아마존 웹 서비스로 발표 되었다는 느낌이다. 



아래는 발표된 서비스에 대한 간략한 설명과 자세한 사용을 위한 링크다. 



EC2 확장 기능, Elastic GPU : https://aws.amazon.com/ko/blogs/korea/in-the-work-amazon-ec2-elastic-gpus/

- EC2 서버 인스턴스에 GPU를 붙였다 떼었다 할 수 있도록 디자인된 EC2 확장 기능 



F1 인스턴스 패밀리 : https://aws.amazon.com/ko/blogs/korea/developer-preview-ec2-instances-f1-with-programmable-hardware/ 

- FPGA를 사용할 수 있는 새로운 인스턴스 타입 



Amazon Athena : https://aws.amazon.com/ko/blogs/korea/amazon-athena-interactively-query-petabytes-of-data-in-seconds/

- S3에 저장된 데이터에 표준 SQL을 사용할 수 있는 서비스 



Amazon LEX : https://aws.amazon.com/ko/blogs/korea/amazon-lex-build-conversational-voice-text-interfaces/ 

- 음성 인식 및 자연어 이해를 통해 채팅봇 또는 애플리케이션에 음성 인터페이스를 추가할 수 있도록 하는 신규 서비스. 아직 한국어 지원은 되지 않는 듯 



Amazon Polly : https://aws.amazon.com/ko/blogs/korea/polly-text-to-speech-in-47-voices-and-24-languages/ 

- 다양한 음색과 언어를 지원하는 문장 -> 음성 인터페이스 서비스. 지원하는 언어에 사용되는 다양한 어휘를 상황에 따라 올바르게 읽는 능력을 제공, 따라서 LEX 와 함께 사용할때 애플리케이션에서 처리한 내용을 음성으로 사용자에게 전달할 수 있음 



Amazon Rekognition : https://aws.amazon.com/ko/blogs/korea/amazon-rekognition-image-detection-and-recognition-powered-by-deep-learning/ 

- 이미지의 내용을 분석해 주는 서비스. 이미지의 사람 표정에서 감정을 추출하거나, 이미지의 내용을 딥러닝에 기반하여 리턴 



Amazon Aurora - Posgresql : https://aws.amazon.com/ko/blogs/korea/amazon-aurora-update-postgresql-compatibility/ 

- 아마존 오로라는 아마존이 만든 오픈소스 기반의 데이터베이스 서비스인데, 종전에는 MySQL 호환만 존재 하였으나 이번에 Postgres 엔진도 발표 됨. 기존의 RDS Posgres 로 부터의 손쉬운 마이그레이션 등을 제공하며, 일반적으로 두배 정도의 성능 향상을 기대할 수 있다고 함 



AWS Greengrass : https://aws.amazon.com/ko/blogs/korea/aws-greengrass-ubiquitous-real-world-computing/ 

- IoT 관련 서비스를 만들다 보면 데이터를 클라우드로 전달하는 과정에서 누락등이 발생하기 때문에 이를 보완하기 위해 Sensor 게이트웨이 같은 장치를 구현하는 경우가 많은데, 이를 클라우드로 직접 전달하는 대신 센서등이 위치한 로컬의 하드웨어에서 Lambda 기반의 Greengrass Core 를 실행하여 작업을 처리하는 도구. 즉, IoT 데이터 처리를 위한 로컬 람다 소프트웨어. 



AWS Snowball Edge : https://aws.amazon.com/blogs/aws/aws-snowball-edge-more-storage-local-endpoints-lambda-functions/ 

- 내용을 보다 보면 기존의 Storage Gateway 와 Snowball 을 합친 모양새. Snowball 은 로컬의 데이터센터에서 아마존으로 데이터의 이동이 필요한 경우 제공되는 일종의 운반용 디스크 세트였는데, 이를 아예 센터의 로컬에 설치하여 연동할 수 있도록 구현한 것으로 보임. 인터페이스는 10G, 25G, 40Gbps 를 제공하며, 심지어 Zigbee 역시 지원한단다. S3의 API 가 달려있으며 멀티파트 업로드 등으로 데이터 전송을 빠르게 처리할 수 있고, 몇개를 더 묶어서 클러스터링까지 가능한 모양. 람다를 사용한 컴퓨팅 기능까지 포함 하고 있는 로컬 스토리지 + 컴퓨팅 머신  



AWS Snowmobile : https://aws.amazon.com/blogs/aws/aws-snowmobile-move-exabytes-of-data-to-the-cloud-in-weeks/ 

- 엑사 바이트 규모의 데이터 이전에 사용한다. 데이터센터에 아무리 빠른 네트워크가 있다고 하더라도, 엑사바이트 규모의 데이터를 아마존으로 전송하기 위해서는 수개월-1년 이상의 시간이 필요할 수 있다. 이에 아마존은 스노우모바일이라 불리는 서비스를 출시 하였으며, 거대한 컨테이너가 달린 트럭을 사용한다.

{{< responsive-figure src="http://cfile28.uf.tistory.com/image/2573744C583F39E9264AF8" >}}


오늘 나온 서비스들이라서 더 자세한것은 써봐야 알겠지만 어쨌든 클라우드 서비스 시장에서의 맹주로서 무서운 속도로 서비스를 내어 놓는 것 만은 분명하다. 새롭게 늘어난 인스턴스 종류, 직접 구현하기 힘든 다양한 음성 및 데이터 관련 서비스들은 아마도 다양한 스타트업들에서 다양한 형태로 사용될 것으로 생각된다. 

놀라운 점은, 이미 시장에서 선두를 달리고 있는 회사가 이런 속도로 또 다시 신규 서비스들을 발매 한다는 점이다. 아울러 최근의 모든이가 알겠지만, 1.0 버전의 발매는 거기서 끝이 아니라 지속적인 개선을 필요로 한다. 수십개가 넘는 각각의 아마존 웹 서비스의 제품들이 저마다 또 다른 속도로 개선될 것을 생각해 보면, 2017년에는 아마도 일주일에 50개씩 업데이트가 생길지도 모르겠다. ㅎㅎ 

아울러 피보탈의 클라우드 파운더리에서 Amazon Service broker 를 통해 다양한 신규 서비스를 애플리케이션 개발자가 손쉽게 연동해서 사용할 날이 오기를 기대한다. 

(CircleCI Test)

(younjin.jeong@gmail.com, 정윤진)
