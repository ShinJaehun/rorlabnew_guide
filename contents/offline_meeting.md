# Offline Meeting

***Toddling with RSpec*** : 프로그램 실습 모임으로 격주로 진행되며, 페이스북 그룹([ROR Lab Renewal](https://www.facebook.com/groups/rorlabrenewal/))에 이벤트로 공지가 올라 갑니다.

![RSpec과 함께 걸음마를](https://fbcdn-sphotos-c-a.akamaihd.net/hphotos-ak-frc3/t1/1981741_651556861560022_2000921462_n.jpg)


## 목적
* 레일즈 개발을 몸으로 익힌다
* 개인 프로젝트를 공유하거나 막히는 점을 토론한다.
* 로라 API를 개발한다.
* RSpec 공부

## 운영방법
1. 슬랙에서 막히는 부분이 생기거나 로라 API 기능을 개발하기 위한 토론 또는 질문과 답변을 합니다.
1. 페어 프로그래밍을 하기 위해 누구나 사람을 모집합니다.
- 시간과 장소는 주중 평일 저녁(페이스북 조사에서는 화요일에 가능하다는 의견이 많았음), 강남역 토즈(1호점: 강남역 10번출구 주변, 2호점: 신논현역 주변)
1. 모임 참석자가 3인 이상이면 [페이스북 그룹 RoR랩 리뉴얼](https://www.facebook.com/groups/rorlabrenewal/)에 이벤트를 공지합니다.
1. 모이기 전에 만나서 토론하거나 코딩하거나 해결할 미션을 정합니다.
1. 모임 결과를 공유합니다.

***

### 향후 운영 방안 또는 의견
* 누구나 자유롭게 의견을 달아주세요.
* 슬랙에서 자유롭게 토론합시다. 아무거나 좋으니 물어보세요.
* 필드 추가 등 마이그레이션하더라도 롤백할 수 있으니 프로젝트 개발 초기에 여러가지 시도를 해봅시다.
* 모임에 직접 참가하기 어려운 사람들을 위해 클라우드 개발툴로 페어 프로그래밍을 합니다. 예) [c9.io](http://c9.io), [nitrous.io](http://nitrous.io), [페어 프로그래밍 위드미](http://www.pairprogramwith.me/)
* 무선 또는 유선 인터넷으로 행아웃 온에어를 하여 멀리사는 사람도 참여할 수 있도록 도와줍니다. => 인터넷이 느린 경우가 있어서 실시간 중계는 하지 않고 화면녹화하여 공유.
* 라즈베리파이로 미러링하고 행아웃 온에어를 하는 방법을 검토합니다.

### 지난 모임

| 차수 | 일자 | 참석자 | 비고 |
|-----|------|-------|------|
|[1차](https://www.facebook.com/events/1447760818788233/)|2014-02-08(토)|이한국, 김성준|여의도에서 모이기로 했으나 좀 더 공부한 후 만나기로 함.|
|[2차](https://www.facebook.com/events/595232873891002/) | 2014-02-14(금) | 이한국, 정창훈, 박유진, 홍순상, 김성준 |
|[3차](https://www.facebook.com/events/602466783170169/) | 2014-02-26(수) | 정창훈, 박유진, 임영수, 김성준 | 행아웃을 시도했으나 무선인터넷이 잘 끊어져서 실패함. 박유진님과 임영수님의 페어 프로그래밍이 돋보였음.|
|[4차](https://www.facebook.com/events/590367331048059) | 2014-03-11(화) |  최효성, 윤승준, 박유진, 임영수, 이한국, 김성준  | controller spec은 시간부족으로 다음 시간에 |
|[5차](https://www.facebook.com/events/528311210618476) | 2014-03-25(화) | 최효성, 윤승준, 박유진, 홍순상, 정창훈, 임영수, 이한국, 김성준 | "Project 개발 현황" 페이지에서 진행하는 작업을 행 추가해주세요.|
|[6차](https://www.facebook.com/events/456583561141054) | 2014-04-10(목) | 최효성, 윤승준, 임영수, 박유진, 김대권, 아샬, 홍순상, 김성준 | 코드 리뷰(Bulletin, Event, QnA), 콘트롤러 스펙에 대한 라이브 코딩, 로라API 모델링 |

| 차수 | 내용 | 자료 |
|-----|------|------|
|[1차](https://www.facebook.com/events/1447760818788233/)| | |
|[2차](https://www.facebook.com/events/595232873891002/) | RSPEC 초기 설정과 모델 스펙 작성 공부(이한국님의 "Everyday Rails Testing with Rspec" 책을 읽고 슬라이드로 지식 공유)|녹화 [#1](http://youtu.be/PJifosyrYSE), [슬라이드](http://slid.es/majestin/rorlabtdd)|
|[3차](https://www.facebook.com/events/602466783170169/) | 포스트(Post) 모델 스펙의 예문("it" example) 작성, 포스트 필드 마이그레이션(추가), 포스트 팩토리 작성, | 녹화 [#1](http://www.youtube.com/watch?v=2-kUBhXW3y0&feature=share) [슬라이드](http://slid.es/wagurano/20140304) |
|[4차](https://www.facebook.com/events/590367331048059) | [RSpec matcher 소개](https://relishapp.com/rspec/rspec-expectations/v/2-14/docs/), [커밋 로그 정리하는 방법](http://git-scm.com/book/ko/Git-%EB%8F%84%EA%B5%AC-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EB%8B%A8%EC%9E%A5%ED%95%98%EA%B8%B0) | 녹화 [#1](http://youtu.be/0Xx1Lce51eM), [#2](http://youtu.be/V6tzu9N5fek), [슬라이드](https://docs.google.com/presentation/d/1W-qIfc8Rb7MlCNLb7PwceZeeuQGxvmSurMuqAuhzQwM/), [예시](https://github.com/plaredspear/ror_tdd_example) |
|[5차](https://www.facebook.com/events/528311210618476) | 1부. 임영수님의 RSpec 매처(matcher) 실습 강의, 2부. 코드 리뷰, 3. 실습 서버 신청 | [플라자, 포스트 모델 리뷰](http://slid.es/majestin/rorlabtdd2), [모임 후기(작성중)](https://docs.google.com/document/d/1kF0L-hBepSqTJts66AZKNiCBlJECJ83JYqt6nzkL5XA/edit?usp=sharing) |
|[6차](https://www.facebook.com/events/456583561141054) |  | 후기 작성중 |
