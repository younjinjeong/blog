---
authors:
- younjin
categories:
- News
- AWS 
- reInvent
date: 2016-11-19T06:34:12-04:00
short: |
  2016년 AWS re:Invent 키노트 둘째날 소개된 서비스들에 대한 간략한 정리   
title: AWS re:Invent 2016 Keynote 2, New services 
---

(younjin.jeong@gmail.com, 정윤진) 

어제에 이어 오늘도 새로운 서비스들이 쏟아져 나왔다. re:Invent 첫째날은 아마존 웹 서비스의 사장인 Andy Jassy 가 발표를 했고, 두번째 날에는 아마존의 CTO인 Werner Vogels 박사님이 또 새로운 서비스들을 쏟아내었다. 죽 보면서 느끼는건데, 대체 각 서비스별로 Amazon 과 AWS의 이름 규칙은 무엇인가. 알수가 엄쏭. 

Keynote 첫날 발표된 내용은 여기로. : http://blog.younjinjeong.io/post/aws-reinvent-2016-keynote-part-2/


{{< responsive-figure src="http://cfile27.uf.tistory.com/image/237CB6505840A1A036188A" >}}

2일 동안 발표된 서비스 및 추가 기능은 무려 24개에 달하는데 이는 역시 아마존이 시장의 선두를 유지하기 위해 박차를 가하는 모습이다. 대부분의 서비스들은 기존 고객의 요구 또는 고객들이 아마존 웹 서비스에서 주로 직접 구현해서 사용하는 것에 어려움을 겪는 문제를 해소하기 위해 매니지드 서비스로 만들어 배포하고 있다. 



두번째 날에 공개된 서비스들은 주로 개발자들이 관심을 가질만한 내용들로 보인다. 아래에 간단히 그 용도와 링크를 적어본다. 한국에 대한 아마존 웹 서비스의 투자가 늘어나 re:Invent 에서 발표된 내용이 실시간으로 한글 블로그로 번역되어 나오는것에 신기할 따름이다. 



AWS X-Ray : https://aws.amazon.com/ko/blogs/korea/aws-x-ray-see-inside-of-your-distributed-application/  

- 웹 애플리케이션에서 클라이언트로 부터 서비스로 들어온 요청이 내부에서 어떻게 처리되는지에 대한 가시성을 확보하는 것은 매우 중요하다. 특히 마이크로 서비스와 같은 구조를 구현하고 있는 경우라면 더더욱 그러한데, 이를 AWS에서 사용할 수 있는 것이 X-ray 라는 서비스로 발매 되었다. 트위터가 만든 오픈소스 Zipkin (http://zipkin.io) 의 AWS 버전으로 보인다. 



AWS CodeBuild : https://aws.amazon.com/ko/blogs/korea/aws-codebuild-fully-managed-build-service/ 

- 아마존의 코드 삼형제라는 별명을 붙여서 한참 발표하고 다녔는데 이제 코드 4형제가 되었다. AWS가 만든 Jenkins 라고 이해하면 되겠다. AWS CodeCommit, AWS CodePipeline, AWS CodeDeploy 와 연동 가능하며 특히 CodePipeline 에서 빌드 서비스 공급자로 CodeBuild 를 추가할 수 있어 유용하겠다. 사실 CI/CD 파이프라인 및 컨테이너 레벨의 빌드와 테스트를 수행하는 도구를 직접 설치해서 운영하는 것은 매우 피로도가 높은 일인데, 이렇게 서비스로 제공되면 사용이 편리하겠다. 



AWS Shield : https://aws.amazon.com/ko/blogs/korea/aws-shield-protect-your-applications-from-ddos-attacks/

- 많은 사람들이 아기다리고기다리던 기능이 아닐까 싶다. 아울러 이쪽 분야에 솔루션을 판매하던 보안회사들 몇몇은 곡소리가 날지도... 아무튼 AWS 의 DDoS 방어 서비스다. 설명에 따르면 Standard 서비스는 모든 사용자가 무료로 사용할 수 있으며, 지원을 받고자 하는 경우에는 Advanced 를 선택 가능한 모양이다. DDoS 방어의 경우 도메인 부터 서비스 엔드포인트까지 다계층에서 감지 및 조치를 취해야 할 필요가 있는데, Shield 서비스의 경우 Route53 의 도메인, CloudFront 의 CDN edge, 그리고 ELB의 밸런서의 다계층에서 DDoS를 방어한다. 모든 사용자가 살펴볼 필요가 있는 서비스. 



AWS Pinpoint : https://aws.amazon.com/blogs/aws/amazon-pinpoint-hit-your-targets-with-aws/

- 마케팅에서 선별된 고객에게만 메세지를 보내는 기능은 매우 중요하다. AWS Pinpoint 는 Mobile analytics 에서 제공하는 몇몇 간단한 지표를 바탕으로 특정 조건에 부합하는 고객들에게 SNS(Simple Notification Service) 를 통해 푸시알람이나 이메일등을 보낼 수 있는 마케팅 그룹용 매니지드 서비스라고 할 수 있겠다. 어떠한 모바일 서비스건 꼭 구현해야 하는 기능이지만 직접 구현하려면 시간과 노력이 필요한 기능을 서비스로 만들었다. 



AWS Batch : https://aws.amazon.com/ko/blogs/korea/aws-batch-run-batch-computing-jobs-on-aws/ 

- 배치 작업의 종류는 다양하지만, 그 기본 구성요소는 거의 대부분 비슷하다. 작업을 위한 클러스터와 이 클러스터를 통제하는 메인 노드 (또는 서버) 그리고 클러스터가 수행할 작업을 분배하고 결과를 수집하는 작업 큐, 그리고 그 클러스터의 수행 목적에 따라 결과를 저장하는 데이터베이스 또는 파일 시스템으로 보통 구성한다. Batch 서비스는 이전 MIT의 StarCluster 와 같은 도구가 하던 일을 아예 서비스로 만들고, 데이터 소스 및 결과물 저장을 위한 DynamoDB, S3 등과의 연동 및 작업을 위한 클러스터 노드 생성과 큐를 만들어 그야말로 뚝딱 사용할 수 있는 서비스다. 예를 들면 렌더링이나 분자화학식 계산, 각종 산업 시뮬레이션 등에 사용 가능하겠으며, 아마도 GPGPU 가 달린 또는 Elastic GPU 등을 사용해 OpenCL 등을 사용하는 클러스터도 제공할 것 같다. 



AWS Personal Health Dashboard : https://aws.amazon.com/ko/blogs/korea/new-aws-personal-health-dashboard-status-you-can-relate-to/ 

- AWS에서 서비스를 운영하다 보면 관리를 위한 정보를 각 서비스 콘솔에서 별도로 확인해야 하는 등의 불편함이 있었다. 예를 들면 메인터넌스를 위한 AWS의 EC2 인스턴스 재부팅 대상이라던가, RDS 데이터베이스의 업데이트 또는 CloudWatch 를 통해 확인해야 하는 각종 알람과 같은 정보들이다. 이런 정보들을 이제 여기저기 돌아다닐 필요 없이 하나의 대시보드에서 확인할 수 있도록 제공하는 서비스로 보인다. 



Blox, OSS Container scheduler  : https://aws.amazon.com/ko/blogs/korea/blox-new-open-source-scheduler-for-amazon-ec2-container-service/ 

- 어째서 ECS에서 사용하는 컨테이너 스케줄러를 서비스로 제공하지 않고 오픈소스 도구로 처리하라는지는 잘 모르겠지만, 어쨌든 그렇다. 아마존 웹 서비스를 사용하다 보면 EC2도 그렇고 현재 내가 어떤 인스턴스들 또는 컨테이너를 동작하고 있는지 Describe 관련 API 를 호출하는 경우가 많다. 이는 각 컨테이너의 "State - 상태" 를 그때그때 조회해야 하기 때문에 여간 불편한 것이 아닌데, 이 Blox 라는 도구는 CloudWatch 등을 통해서 상태를 추적하고 사용자가 원하는 수량 또는 작업을 SQS 를 통해 처리하도록 하는 오픈소스란다. 난 피보탈 직원이니까 컨테이너를 클라우드에서 돌리고 싶은 엔터프라이즈라면 그냥 피보탈 클라우드 파운더리를 살펴보는걸 권고. 데헷 



AWS Lambda@Edge : https://aws.amazon.com/ko/blogs/korea/coming-soon-lambda-at-the-edge/ 

- AWS 람다는 함수 단위의 코드를 서버 없이 실행해 주는 서비스다. 이 서비스가 이제 CloudFront의 Edge에서 돌아간단다. 그러니까 즉, CDN에 컴퓨팅 파워를 부여해서 외부에서 들어오는 요청에 대해 더 스마트한 통제를 구현할 수 있게 된다. 현재는 자바스크립트 코드만 동작 가능한 것으로 보이는데, 정리하면 웹 서버 또는 NginX 와 같은 리버스 프락시에서 구현하는 다양한 웹 요청에 대한 조작을 CDN 엣지에서 내가 원하는 대로 자바스크립트를 사용해서 처리할 수 있겠다. 다양하게 응용이 가능하므로 웹 서비스를 수행한다면 꼭 살펴 보아야할 기능. 



AWS StepFunctions : https://aws.amazon.com/ko/blogs/korea/new-aws-step-functions-build-distributed-applications-using-visual-workflows/

- 서버리스가 점점 발전하고 있다. Lambda 에서 작성한 각각의 함수 레벨의 애플리케이션을 서로 엮어 개발자가 원하는 조건에 따라 원하는 동작을 처리할 수 있는 일종의 "람다 함수를 묶어 하나의 서비스로" 와 같은 컨셉. 아마존에서 주창하는 서버리스와 람다에 열광하는 사용자라면 반드시 살펴볼 것을 권고한다. 



AWS Glue : https://aws.amazon.com/ko/glue/ 

- 아마존의 공식 블로그 포스팅은 아직 없는 모양. 서비스 홈 페이지를 링크. 간단히 말해서 ETL as a Service. JDBC 로 연결 가능한 모든 데이터 저장소와 S3, RDS, Redshift 간의 데이터 이동등을 편리하게 처리할 수 있겠다. 또한 이렇게 만들어진 ETL을 Python, Spark, Git 또는 IDE 등을 사용해서 다른 Glue 사용자들과 공유를 할 수도 있단다. 따라서 Kinesis firehose 등을 사용하여 인입된 데이터를 서비스간 원하는 대로 편리하게 쪼물락 할 수 있다는 말. 



몇가지 서비스가 더 있기는 하지만, 일단 영문과 국문 블로그에 소개된 서비스들을 우선으로 정리 했다. 어차피 다들 시간을 두고 차근차근 살펴보아야 할 서비스들이므로, 몇개월 재미지게 생겼다. 

매번 느끼는 거지만 최근의 비지니스들은 모두 인터넷과 소프트웨어 그리고 데이터를 사용하지 않는 경우는 드물다. 더 중요한 것은 동일한 작업의 반복, 예를 들면 주로 테스트나 빌드, 그리고 배포등 과 같이 사람이 하면 오래걸리거나 실수가 발생하여 장애로 발전하는 형태의 것들이 클라우드 시대에 더더욱 많아진다. 금번 아마존 웹 서비스의  re:Invent 를 보면 인프라는 내게 맡기고 너는 서비스에 집중하라 는 메세지가 매우 강하다. 탄탄한 기본 서비스들을 바탕으로 점점 발표되는 신규 서비스의 양이 가속화 되는 것을 보면 굉장하다. 

나 역시 아마존 웹 서비스의 강렬한 팬이지만, 아마존 웹 서비스를 사용한다고 아마존 닷 컴을 만들 수 있는 것은 아니다. 결국 핵심 역량은 서비스에 필요한 애플리케이션과 데이터 코드를 얼만큼 빠르게 개발하고 테스트해서 배포할 수 있는가 하는 것이 아닐까.  



아마존 웹 서비스의 시장과 고객에 대한 집착이 보이는 굉장한 2일의 키노트였다. 

(younjin.jeong@gmail.com, 정윤진) 