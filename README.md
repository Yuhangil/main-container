# team[-2] 응애 나 애기 멘티



## 프로젝트 기획 이유


![third](./third.png)



## 주요기능 설명
1) 멘토링 키워드 알림
  + 키워드를 등록해두고, 이와 관련된 멘토링이 개설되면 멘토링 정보와 신청 버튼이 포함된 메시지 발송
  + 키워드 등록은 메시지의 모달로 전달
  (직접 구동한 사진)
2) 멘토링 신청 및 취소
  + 소프트웨어 마에스트로 ID & PW를 등록하고 1)의 기능으로 전달된 멘토링을 신청후 결과를 메시지로 발송
  + 이 기능을 통해 신청한 멘토링을 다시 취소하는 것 역시 가능
  + 계정 정보는 user_id로 매핑하여 연계된 DB에 저장됨
  + (멘토링을 신청하고 실제로 신청된 사진)
3) 멘토링 일정 관리
  + 신청한 멘토링은 연계된 DB에 저장되며 이를 기반으로 일정 표시
4) 멘토링 후기 및 한줄평
  + 멘토링별 수강자 목록을 스크래핑하여 멘토링 한줄평 요청 메시지 발송
  + 모달을 통해 DB에 업로드하여 저장
  + 위 내용을 기반으로 멘토와 멘토링에 대한 한줄평 열람 가능
  +(구동 예시)

## 아키텍쳐 구조
![아키텍쳐쳐](./아키텍쳐쳐.png)
------------
## API Document
### 내부 API(Goorm Container)
1) Alert Message
  + (함수 예시)
2) Request Message
  + (함수 예시)
3) Callback Message
  + (호출 예)
### 외부 API(Aws Lamda Function)
1) Upload Alert(게시물 업로드 알림)
  + 10min 마다 게시물 데이터를 읽고 새로운 데이터가 있으면 Altert Message API 호출
  + req.body:
```
[{data}...]
```
  + 예시
```
data=
{
    "dataType": "mentoring",
    "id": 590,
    "subject": "[자유멘토링]5/7(금)_18:00부터-오선식팀 멘토링",
    "location": "온라인",
    "link": "https://swmaestro.org/sw/mypage/mentoLec/view.do?qustnrSn=590&menuNo=200046",
    "status": "[접수중]",
    "period": "2021-04-25 ~ 2021-05-06",
    "date": "2021-05-07",
    "applicants": "1",
    "mento": "최원서",
    "description": "[자유멘토링]5/7(금)_18:00부터-오선식팀 멘토링"
  }
```
2) Apply(멘토링 신청)
  + method : post
  + URL : https://e79d0h6thd.execute-api.us-east-2.amazonaws.com/default/swm-applicant
  + header
    + Content-Type : apllication/json
    + x-api-key : api key
  + body
  + req 예시
```
{

"link":"https://swmaestro.org/sw/mypage/mentoLec/view.do?qustnrSn=585&menuNo=200046",

"userId":"1234444",

"type" :"applicant"

}
```
  + res 예시
```
{

"link":"https://swmaestro.org/sw/mypage/mentoLec/view.do?qustnrSn=585&menuNo=200046",

"userId":"1234444",

"type" :"applicant"

}
```

```
{
    "statusCode": 400,
    "body": "wrong type" //type 틀렸을때
}
{
    "statusCode": 400,
    "body": "계정정보가 존재하지 않습니다" //db에 계정정보 없을 때
}
{
    "statusCode": 500,
    "body": "{'message':'신청을 실패했습니다','number':-1'}" //홈페이지에 버튼없어서 실패
}
```
4) Cancel(멘토링 취소)
  + method : post
  + URL : https://e79d0h6thd.execute-api.us-east-2.amazonaws.com/default/swm-applicant
  + header
    + x-api-key : api key
  + body 
    + link : 신청할 멘토링 게시글의 URL
    + userid : 카카오워크 id
    + type : 신청 or 취소
    + req 예시
```
{

"link":"https://swmaestro.org/sw/mypage/mentoLec/view.do?qustnrSn=585&menuNo=200046",

"userId":"1234444",

"type" :"cancle"

}
```
6) Review(멘토링 후기)
------------


## 팀원 목록
+ 임동진(팀장)
+ 이원기
+ 이선노
+ 함초롬 
+ 김호준
+ 유한길(문서화)

