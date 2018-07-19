# Dialogflow
## 이 문서에 제대로 포함되지 않은 내용 
Entity, Dialog 관련 내용, 공식 문서 링크로 대체하였음 (영문)

## Dialogflow 란?
자연어 처리 엔진을 이용한 의도 및 성분 분석 서비스

## 구성요소
### Agent
Intent, Entity 의 조합으로 이루어지는 NLU (Natural Language Understanding) 모듈

사용자의 요청(String)이 내부 Intent 중 하나와 일치할 때 실행 가능한 데이터로 변환할 수 있음

아래 사용자 요청 처리 다이어그램 중 Dialogflow 컴포넌트가 Agent에 해당함

![AgentOverview](https://dialogflow.com/docs/images/overview/agents/overview.png?dcb_=0.633336241292979)

### Intent
유저 입력에 대해 어떤 작업을 수행할지에 대한 것을 매핑함

Intent 내 설정은 Context, Event Training Phrases, Action & Parameter, Response, Webhook 으로 나뉨

#### Context
유저 입력의 문맥 정보를 나타냄

custom context 를 지정하면 유저 입력을 한 Intent에서 처리한 뒤 다른 Intent로 넘겨 대화를 이어갈 수 있음

##### Input / Output Context

![Context](https://dialogflow-dot-devsite-v2-prod.appspot.com/docs/images/overview/contexts/contexts-002.gif?dcb_=0.21903900849387825)

여러개의 input, output context 를 지정할 수 있음

Input Context 가 다른 Intent 의 Output Context 인 경우, 그 Intent 로 부터 대화가 넘어올 수 있음을 의미함

input context 가 여러개인 경우에는 해당 context 가 모두 매칭되지 않으면 그 intent 로 대화가 넘어갈 수 없음

##### Lifespan
Context 에 수명을 지정하여 활성화 되고 난 뒤 요청을 몇개나 더 받을지, 혹은 몇 분동안 더 활성화 시키고 있을지를 지정할 수 있음

아래와 같이 수명이 0 으로 설정된 경우에는 해당 Intent 에 매칭되었을 때 리셋이 됨
![ContextSetZero](https://dialogflow.com/docs/images/overview/contexts/contexts-003.png?dcb_=0.08211054449693012)

#### Event
트리거 역할을 할 이벤트를 지정함

미리 만들어진 Event 를 사용할 수도 있음 (Welcome, FACEBOOK_LOCATION, ...)

##### Custom Event
요청을 통해 특정 Intent 를 바로 트리거 시키도록 custom event 를 지정할 수 있음 
- query string 은 포함하지 않아도 됨 (자연어 해석이 비활성화됨)
- parameter pair (key, value) 를 포함해야 함


#### Training Phrases
Agent 를 미리 훈련시키기 위한 문장들을 지정함

예상되는 입력들을 설정하여 유저 입력을 받았을 때 Intent 매칭 및 Parameter 화의 정확도를 높일 수 있음
#### Action & Parameter
파라미터와 취할 액션을 지정함

Entity 를 설정하여 유저가 입력한 문자열에서 파라미터로 뽑아낼 정보를 지정하거나, Entity 설정 없이 고정된 Value 를 파라미터로 지정할 수 있음

##### Required & Prompt
- Required : 필수 파라미터 지정
- Prompt : 원하는 정보를 얻지 못했을 때의 응답을 지정 (Required 선택 시 나옴)

#### Response
유저 입력에 대한 응답을 지정함

Action & Parameter 에서 지정한 현재 Intent 의 파라미터, Context 로부터 넘어온 다른 Intent 파라미터를 사용하여 응답형태를 지정할 수 있음

텍스트 뿐만 아니라 이모지, 이미지 등을 응답으로 지정할 수도 있으며 Google Assistant, Slack 등과 연동하여 검색 결과와 같은 카드형 응답도 사용할 수 있음 (Rich Messages)

##### Using Parameter
- Action & Parameter 에서 지정한 현재 Intent의 파라미터 
`$[parameter_name]`
- Context 로부터 넘어온 다른 Intent의 파라미터
`#[context_name].[parmeter_name]`


### Entity
유저 입력에서 파라미터를 뽑아내는데에 사용되는 개체들

자세한 내용은 [여기](https://dialogflow.com/docs/entities)를 참조 

### Fulfillment
유저 입력과 매칭된 Intent 의 정보를 웹 서비스로 전달하여 추가적인 서비스 로직을 실행시킬수 있음

웹 서비스로의 요청은 POST 요청으로 json 형식의 데이터를 보냄

#### Limits
- 서비스 응답에 대한 타임아웃 : 5초
- 서비스로 부터 받을 수 있는 응답의 크기 : 64K

#### Authentication
두가지 방법으로 인증할 수 있음
- ID 와 Password를 사용하는 인증 (로그인)
- 추가적인 인증 헤더를 사용하는 인증

연동된 서비스에 인증이 필요하지 않다면, 인증 필드를 공란으로 둠

서비스는 HTTPS를 사용해야만 하며, Public URL 접근이 가능해야만 함

#### Slot Filling
Intent 내의 "Enable webhook call for during slot filling" 기능을 활성화 시키면 Parameter를 채우기 위해 웹 서비스를 이용할 수 있음

#### Firebase
간단한 Webhook 테스트 및 구현을 위해 Fulfillment 페이지에서 Firebase 를 사용하도록 설정할 수 있음

Inline Editor 를 이용하여 Firebase 의 코드를 수정할 수 있음

#### Request & Response
요청 및 응답 필드, 예제는 [여기](https://dialogflow.com/docs/fulfillment)를 참조

## Dialog
인터랙션 시나리오를 짜기 위해 Linear 혹은 Non-Linear 두가지 방식을 고려할 수 있다.

자세한 내용은 [여기](https://dialogflow.com/docs/dialogs#non_linear_dialogs)를 참조
