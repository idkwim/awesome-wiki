---
title: API 디자인은 왜 중요한가?
date: 2015-01-01 11:49:55
categories: devops
---

이 문서는 모바일 게임 및 결제 플랫폼을 통해서 써드 파티에 제공하기 위한 Java, REST API를 개발해오면서 API 디자인과 버전 관리에 대한 개인적인 경험을 정리하고자 작성하였다.

#### 좋은 API 디자인은 왜 중요한가?

API는 회사 또는 조직이 가지는 중요한 자산이다. 대부분 외부의 클라이언트가 사용하게 되며 한번 배포되면 디자인이 완료된 API는 수정하기 쉽지 않기 때문에 더욱 신중하게 설계되어야 한다.

#### `Public API는 영원하다. 그것을 제대로 만들 기회는 단 한번 뿐이다.`

그 뿐만 아니라 좋은 API를 개발하기 위한 노력은 프로그래머 자신에게도 아주 좋은 영향을 미친다. 우리는 Public API를 개발하지 않더라도 재사용을 위해 기능을 모듈화하고 이런 기능들은 API를 갖게 되기 마련이다. 좋은 방향으로 재사용되어 지는 이러한 API들은 프로그래머들에게 코드 퀄리티를 높히기 위한 동기부여가 된다.

#### 그렇다면 좋은 API는 무엇일까?

API가 우선적으로 필요한 전제는 기능에 대한 요구사항을 충족해야 한다는 것이다. 여기에 사용자에게 좋은 API를 제공하기 위해 고려해야 점들은 아래와 같다.

- 다시 한번 강조하지만 API는 기본적으로 요구사항을 충분히 만족시켜야 한다.
- 배우기 쉬어야 한다. 
- 문서가 없어도 사용하기 쉬어야 한다. 문서보다 API가 적용된 예제 또는 샘플 애플리케이션을 제공하는 것이 더욱 효과적이다.
- 잘못 사용하기 어려울 것. 만약 사용자가 API의 의도대로 움직이지 않는다면 쉽게 어떤 부분이 잘못 되었는지 알려줄 수 있어야 한다.

## API의 디자인에 필요한 원칙들

#### API 디자인 프로세스

- 짧은 스펙으로 시작하는 것이 좋다. 처음부터 완벽하고 많은 설계를 시작하는 것보다 프로토타입 애플리케이션에 간단한 기능을 적용하는 API를 개발하는 것으로 시작하라.
- API에 대한 글을 자주 적는 것이 좋다. API가 필요한 이유를 시작으로 해결해야하는 문제를 지속적으로 정의하라.
- Public API의 내부는 언제든지 유지할 수 있다. 사용자 관점에서 API를 디자인해라. TDD를 통해 좋은 디자인을 찾아낼 수 있을 것이다.
- 많은 실수에 미리 노출 될 수 있도록해라. 테스트 코드를 많이 만들고 안정적인 버전이 배포되기 까지는 많은 사용자에게 노출시키는 것이 좋다. 실수를 통해 API를 발전되어 간다.

#### 일반적인 원칙들

- API는 하나의 일만 해야하지만, 그것을 아주 잘 해야 한다. 

- API의 이름은 매우 중요하다. 프로그래밍 언어에 맞는 Convention을 지키고 일관적으로 API의 이름을 짓도록 한다.

- 모든 것을 최소화해야 한다. private 키워드를 통해 불필요한 클래스나 메소드가 사용자에게 노출되지 않도록 한다. 이것은 사용자를 혼란스럽게 만든다.

- 문서화가 중요하다.

#### 클래스 디자인

- 클래스의 변경을 최소화해야 한다.

- 만약 예외적인 일이 없다면 클래스는 Immutable 해야 한다.

- 상속을 위해 설계하고 문서를 남겨야 한다. API가 상속을 위한 설계가 되지 않았다면 상속을 할 수 없도록 만들어라

#### 함수 디자인

- API 내의 기능이 할 수 있는 것을 클라이언트가 하게 만들지 마라.

- 사용자를 당황하게 만들지 말아라

- 빨리 실패하게 만드는 것이 좋다. 장애가 발생되면 가능한 빨리 에러를 사용자에게 알려야 한다.

- 오버로딩을 제공해야 한다면 동일한 행동이 일어나야 한다. 주위를 가지고 오버로딩한다. 동일한 Argument 숫자가 없도록해야 한다. 

- 예외처리를 요구하는 리턴값을 만들지 마라. `[]' 또는 비어있는 컬렉션을 리턴하는 것이 좋다. 널 값을 리턴하지 마라.

#### 예외처리에 대한 디자인

- 예외 상황을 표시하기 위해 exception을 던져라. 클라이언트가 프로그램 흐름을 위해 예외를 사용하도록 해서는 안된다.

- 반대로 조용하게 실패가 일어나면 안된다.

- 예외 안에 에러에 대한 정보를 포함시켜라. 진단을 허용하고 repair 하거나 recovery 하라.

- Unchecked Exceptions를 사용하라. Checked Exceptions는 사용자가 액션을 취해야 한다.

## API의 Version 관리

공개 API(Public API)는 배포되는 시점부터 외부로부터 의존성을 가지게 되는 소프트웨어이다. 의존성이 높은 시스템에서는 새로운 버전을 배포하고, 적용하는 일이 끔찍해 지고는 한다. 어플리케이션 내의 의존성 문제를 효율적으로 관리하기 위해 아래와 같은 버젼 관리 체계를 통해 API가 적용된 어플리케이션와 효율적인 커뮤니케이션이 필요하다.

이 규칙들은 기존 오픈소스에 널리 활용되는 규칙을 바탕으로 했으며, 반드시 그 제약을 따르지는 않는다. 우리가 제공하는 시스템이 동작하기 위해서는 공개 API를 선언해야 하며 배포 이후에 추가 또는 변경되는 API의 내역을 아래와 같은 버전 체계를 통해 어떠한 의미로 변경 되었는지 알려야한다.

REST API 또는 SDK, 라이브러리의 Version 관리 및 배포 결과물을 위해 [Semantic Versioning](http://semver.org) 체계를 준수 한다.

#### Semantic Versioning Summary

`Examples`
```
mobill-sdk-dist-1.0.0
mobill-sdk-facebook-1.0.0
```

- 버전 번호는 X.Y.Z 의 형태로 하며, 각 X,Y,Z 는 자연수(음이 아닌 정수) 이고, 0이 앞에 붙지 않는다. X 는 주 버전 번호이고, Y는 부 버전, Z는 수 버전을 지칭한다.
- 기존 버전과 호환되지 않게 API 가 변경 되면, 주 버전을 업데이트 한다.
- 기존 버전과 호환되면서 새로운 기능을 추가 할 때에는 부 버전을 업데이트 한다.
- 기존 버전과 호환되면서 버그를 수정 했다면, 수 버전을 업데이트 한다.
- 주버전 0.Y.Z 는 초기 개발을 위해서 사용하며, 상시로 변경 가능한 버젼이다. 이 공개 API는 안정판으로 보지 않는게 좋다.
- 1.0.0 버전은 안정화 상태의 공개 API를 지칭한다. 1.0.0 배포 이전 관련 문서 정보를 동기화 시킬 수 있도록 한다.
- 정식 배포 버전에는 기존 관리 버젼과 구분하기 위해 dist prefix를 사용 한다.
- 각 버젼별 Branches 버전 관리는 필요하다면 prefix를 첨부해 구분하도록 한다.

#### 프로젝트의 단계별 관리

1. 프로젝트 버전의 생명 주기

- 특정 버전의 배포를 위해서는 `요구사항 수집 - Issue 등록 - 개발 진행 - 테스트 - 코드 리뷰 - Merge to Master - 빌드 및 배포 단계`를 거치게 된다.
- 특정 버전을 바탕으로 한 새로운 브랜치 버전을 효율적으로 관리 할 수 있어야 한다.
- 특정 버전의 생명 주기의 단계별로 관리가 가능 해야 한다.

2. 특정 버전의 단계별 관리

카테고리 | 설명 
--|--
Master | 프로젝트의 가장 중심이 되는 디렉토리로서 모든 개발 이슈들을 최종적으로 Master에 Merge 될 수 있도록 한다.
Branches | 서비스를 운영 하다 보면, 특정 버전으로부터 파생되는 버전을 관리 해야하며 Master와는 상이한 Issue가 적용 된다.
Tag | 테스트 및 QA 완료 이후 배포 버젼을 관리하기 위한 디렉토리 이며, Tag Version 생성 이후에는 해당 디렉토리는 Read-Only 상태를 유지 한다.

3. Tag 릴리즈 이후 Semantic Versioning 체계를 준수해 Version을 업그레이드 한다.

- 업데이트된 새로운 Version을 부여
- 배포내역에 대한 히스토리를 관리한다.
- 관련된 문서는 동기화한다.

4. Trunk / Branches / Tag 와의 관계

<img src='http://photo.toast.com/aaaacd/images/version.png' />


Branches를 포함한 프로젝트 전반의 공통적인 변경 내역은 리뷰 후에 최초 Master에 적용하도록 하며, 이후 Merge가 필요한 내역은 각 Branches에 Merge 하도록 한다. Master와 상이한 내역을 제외하고, 공통 내역은 각 별도의 Branches에서 먼저 적용 하지 않도록 한다.

#### 빌드 및 배포

- 개발 완료 이후에 새로운 Version 을 부여 한다.
- Master -> Tags/0.Y.Z 으로 Tagging 한다.
- 빌드 환경에서 Tags/0.Y.Z 을 Checkout 후 빌드 및 배포

#### Tag Version에 버그 픽스등의 패치가 필요하다면

- 기존 배포된 Tags version 에 대한 변경이 필요하다면, Tags version을 수정 하지 않는다.
- Tags Version 에서 새로운 Version을 부여하여 Branches 에 Tagging 한다.
- Branches -> Tag/0.Y.Z2 으로 Tagging 한다.
- 빌드 환경에서 Tags/0.Y.Z2 을 Checkout 후 빌드 및 배포


#### 문서 관리

프로젝트 Tag Version 과 동기화 되도록 개발 문서 및 API Reference를 관리한다.

1. 프로젝트와 연관된 문서와 같은 산출물을 Version History 별로 관리 할 수 있도록 한다.
2. 문서의 종류
	- 아키텍쳐 문서
	- 어플리케이션 문서
	- 기타 참조 문서
	- 배포 시 SDK 가이드 문서
3. 문서의 배포
	- 위키 페이지 또는 웹 페이지
	- JavaDoc
	- PDF

#### 개발 가이드 문서

- API의 전반적인 설명과 함께, API를 이용한 애플리케이션의 개발을 위한 환경 설정 및 SDK API에 대한 내용을 기술한 문서 이다.
- API를 적용한 샘플 어플리케이션에 대한 내용을 첨부한다.
- API의 변경 내역을 기술한다. 
- API가 업데이트 되면 관련 문서는 동기화 되어야한다.

#### API References

- API References는 REST API, SDK에서 제공되는 API의 상세 명세를 기술 한다.
- API References는 Tags Version의 최신 상태와 함께 동기화 되도록 관리 하도록 한다.
- API References의 Output 관리는 API를 정의하는 소스에서 산출 될 수 있도록 자동화 도구를 사용한다.
