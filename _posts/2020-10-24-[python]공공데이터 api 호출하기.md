---
title: "공공데이터 api 호출하기"
date: 2020-10-24
categories: python
---
# 공공데이터 api 호출하기
### 비급여진료비정보조회서비스(건강보험심사평가원) api를 호출해 보고, xml로 온 response 값을 json으로 변환 해 보겠습니다.

공공데이터 오픈 API 링크 : https://www.data.go.kr/data/15001700/openapi.do

***

># api 호출

getNonPaymentItemCodeList 에 대해 콜을 해 보겠습니다.

```python
import requests

key = "Dc2%2BZbxPbCMQQj%2FNpy%2FQWJXUH7y4MbfPu9HEdJSdVJ3aOXXPQOWIpw0wNWpf%2FV8YAmRSzPhX4U0NodeC1X2%2B3A%3D%3D"
url = "http://apis.data.go.kr/B551182/nonPaymentDamtInfoService/getNonPaymentItemCodeList?pageNo=1&numOfRows=10&ServiceKey={}".format(key)

content = requests.get(url).content
print(content)
```
requests를 install 해주어야, 콜이 가능 합니다.

결과
```
b'<?xml version="1.0" encoding="UTF-8" standalone="yes"?><response><header><resultCode>00</resultCode><resultMsg>NORMAL SERVICE.</resultMsg></header><body><items><item> ...
```
xml 형태의 응답이 왔습니다.

># xml를 dict 형태로 변환

```python
import requests, xmltodict

key = "Dc2%2BZbxPbCMQQj%2FNpy%2FQWJXUH7y4MbfPu9HEdJSdVJ3aOXXPQOWIpw0wNWpf%2FV8YAmRSzPhX4U0NodeC1X2%2B3A%3D%3D"
url = "http://apis.data.go.kr/B551182/nonPaymentDamtInfoService/getNonPaymentItemCodeList?pageNo=1&numOfRows=10&ServiceKey={}".format(key)

content = requests.get(url).content
dict = xmltodict.parse(content)
print(dict['response']['body']['items'])
```
xmltodict를 install 하고, content를 xmltodict를 이용하여, parse 합니다.

결과
```
OrderedDict([('item', [OrderedDict([('divCd1', 'A'), ('divCd1Dsc', '건강보험에서 정한 비용 외에 추가로 비용부담이 있는 병실입니다. (상급병실을 운영하는 병원은 일정규모의 보험적용 병실을 갖추어야 함) <br>※ 다만, 대형병원(상급종합병원)의 1인실 병실료는 건강보험 제외 대상입니다.  <br>▶ 특실, 출산 관련 병실, 정신과병실은 특수성을 감안하여 정보제공에서 제외하였습니다.'), ('divCd1Nm', '상급병실료차액'), ('divCd2', 'A1100'), ('divCd2Dsc', '1개의 입원실에 1인 입원'), ('divCd2Nm', '1인실')]), OrderedDict([('divCd1', 'A'), ('divCd1Dsc', '건강보험에서 정한 비용 외에 추가로 비용부담이 있는 병실입니다. (상급병실을 운영하는 병원은 일정규모의 보험적용 병실을 갖추어야 함) <br>※ 다만, 대형병원(상급종합병원)의 1인실 병실료는 건강보험 제외 대상입니다.  <br>▶ 특실, 출산 관련 병실, 정신과병실은 특수성을 감안하여 정보제공에서 제외하였습니다.'), ('divCd1Nm', '상급병실료차액'), ('divCd2', 'A1200'), ('divCd2Dsc', '1개의 입원실에 2인 입원'), ('divCd2Nm', '2인실')]), OrderedDict([('divCd1', 'A'), ('divCd1Dsc', '건강보험에서 정한 비용 외에 추가로 비용부담이 있는 병실입니다. (상급병실을 운영하는 병원은 일정규모의 보험적용 병실을 갖추어야 함) <br>※ 다만, 대형병원(상급종합병원)의 1인실 병실료는 건강보험 제외 대상입니다.  <br>▶ 특실, 출산 관련 병실, 정신과병실은 특수성을 감안하여 정보제공에서 제외하였습니다.'), ('divCd1Nm', '상급병실료차액'), ('divCd2', 'A1300'), ('divCd2Dsc', '1개의 입원실에 3인 입원'), ('divCd2Nm', '3인실')]), OrderedDict([('divCd1', 'B'), ('divCd1Dsc', '초음파를 생성하는 탐촉자를 검사부위에 밀착시켜 초음파를 보낸 다음 되돌아오는 초음파를 실시간 영상화하는 검사입니다.<br><br>※ 아래비용은 진단 목적으로 실시하는 검사로 추적검사는 공개비용에서 제외하였습니다.'), ('divCd1Nm', '초음파검사료'), ('divCd2', 'B1100'), ('divCd2Dsc', '물혹, 염증, 양성종양, 악성종양 등의 진단을 위해 실시하는 검사로 추적검사는 공개비용에서 제외하였습니다.'), ('divCd2Nm', '갑상선(부갑상선포함)')]), OrderedDict([('divCd1', 'B'), ('divCd1Dsc', '초음파를 생성하는 탐촉자를 검사부위에 밀착시켜 초음파를 보낸 다음 되돌아오는 초음파를 실시간 영상화하는 검사입니다.<br><br>※ 아래비용은 진단 목적으로 실시하는 검사로 추적검사는 공개비용에서 제외하였습니다.'), ('divCd1Nm', '초음파검사료'), ('divCd2', 'B1200'), ('divCd2Dsc', '물혹, 염증, 양성고형종양, 악성종양 등의 진단을 위해 실시하는 검사로 추적검사는 공개비용에서 제외하였습니다'), ('divCd2Nm', '유방')]), OrderedDict([('divCd1', 'B'), ('divCd1Dsc', '초음파를 생성하는 탐촉자를 검사부위에 밀착시켜 초음파를 보낸 다음 되돌아오는 초음파를 실시간 영상화하는 검사입니다.<br><br>※ 아래비용은 진단 목적으로 실시하는 검사로 추적검사는 공개비용에서 제외하였습니다.'), ('divCd1Nm', '초음파검사료'), ('divCd2', 'B1300'), ('divCd2Dsc', '물혹, 염증, 양성종양, 악성종양 등의 진단을 위해 실시하는 검사로 추적검사는 공개비용에서 제외하였습니다.'), ('divCd2Nm', '상복부(간, 담낭, 담도, 비장, 췌장)')]), OrderedDict([('divCd1', 'C'), ('divCd1Dsc', '양전자단층촬영(PET)은 양전자를 방출하는 방사성 동위원소(F-18 FDG)를 이용하여 인체의 생화학적, 물리학적 영상을 3차원으로 나타낼 수 있는 검사로, 전산화단층영상진단(CT)과 결합된 PET-CT로 검사하기도 합니다. <br><br>※  아래 비용은 검사에 사용된 의약품과 재료 비용을 포함하므로 병원홈페이지의 고지가격과 상이할 수 있습니다. <br>     아울러, 동 검사료 이외에 영상저장 및 전송시스템료(Full PACS)가 추가로 발생할 수 있습니다.<br>   * "비고"의 조영제는 CT 조영제를 의미.'), ('divCd1Nm', 'PET진단료')]), OrderedDict([('divCd1', 'D'), ('divCd1Dsc', '캡슐내시경검사는 기존의 내시경검사로 접근이 곤란한 소장부위의 질환을 진단하기 위해 실시합니다. 평평하거나 침윤이 있거나, 염증이 있는 경우에는 기존의 방사선적 검사로 진단하는데 제한이 있어 이를 보완하기 위해 시행합니다.<br><br>※ 아래 비용은 행위료와 재료대를 포함한 비용이므로 병원의 홈페이지 고지가격과 다를 수 있습니다.'), ('divCd1Nm', '캡슐내시경검사료')]), OrderedDict([('divCd1', 'E'), ('divCd1Dsc', '각종 진단서, 증명서 발급수수료를 말합니다. 일부 진단서는 진찰료가 별도로 발생할 수 있습니다.'), ('divCd1Nm', '제증명수수료'), ('divCd2', 'E1100'), ('divCd2Dsc', '환자의 질병명, 진단날짜, 치료내용, 의사성명 등이 기재<br>※  아래비용은 국문 제증명서 기준입니다.'), ('divCd2Nm', '일반진단서')]), OrderedDict([('divCd1', 'E'), ('divCd1Dsc', '각종 진단서, 증명서 발급수수료를 말합니다. 일부 진단서는 진찰료가 별도로 발생할 수 있습니다.'), ('divCd1Nm', '제증명수수료'), ('divCd2', 'E1200'), ('divCd2Dsc', '개인의 사망을 증명하는 진단서로 사망원인, 사망일시, 사망장소 등이 기재. 의사가 환자를 진료한지 48시간 내에 예견된 원인으로 사망한 경우 발급 <br>※ 아래비용은 국문 제증명서 기준입니다.'), ('divCd2Nm', '사망진단서')])])])
```
json 형태의 응답을 확인 할 수 있습니다. dict['response']['body']['items'] 를 하여, xml 트리의 item 를 가지고 옵니다.

># dict를 json으로 변환

```python
import requests, xmltodict, json

key = "Dc2%2BZbxPbCMQQj%2FNpy%2FQWJXUH7y4MbfPu9HEdJSdVJ3aOXXPQOWIpw0wNWpf%2FV8YAmRSzPhX4U0NodeC1X2%2B3A%3D%3D"
url = "http://apis.data.go.kr/B551182/nonPaymentDamtInfoService/getNonPaymentItemCodeList?pageNo=1&numOfRows=10&ServiceKey={}".format(key)

content = requests.get(url).content
dict = xmltodict.parse(content)
jsonString = json.dumps(dict['response']['body']['items'], ensure_ascii=False)
jsonObj = json.loads(jsonString)
print(jsonObj)
```
json를 install 하고, dumps를 이용하여, json으로 변경 합니다.

결과
```
{'item': [{'divCd1': 'A', 'divCd1Dsc': '건강보험에서 정한 비용 외에 추가로 비용부담이 있는 병실입니다. (상급병실을 운영하는 병원은 일정규모의 보험적용 병실을 갖추어야 함) <br>※ 다만, 대형병원(상급종합병원)의 1인실 병실료는 건강보험 제외 대상입니다.  <br>▶ 특실, 출산 관련 병실, 정신과병실은 특수성을 감안하여 정보제공에서 제외하였습니다.', 'divCd1Nm': '상급병실료차액', 'divCd2': 'A1100', 'divCd2Dsc': '1개의 입원실에 1인 입원', 'divCd2Nm': '1인실'}, {'divCd1': 'A', 'divCd1Dsc': '건강보험에서 정한 비용 외에 추가로 비용부담이 있는 병실입니다. (상급병실을 운영하는 병원은 일정규모의 보험적용 병실을 갖추어야 함) <br>※ 다만, 대형병원(상급종합병원)의 1인실 병실료는 건강보험 제외 대상입니다.  <br>▶ 특실, 출산 관련 병실, 정신과병실은 특수성을 감안하여 정보제공에서 제외하였습니다.', 'divCd1Nm': '상급병실료차액', 'divCd2': 'A1200', 'divCd2Dsc': '1개의 입원실에 2인 입원', 'divCd2Nm': '2인실'}, {'divCd1': 'A', 'divCd1Dsc': '건강보험에서 정한 비용 외에 추가로 비용부담이 있는 병실입니다. (상급병실을 운영하는 병원은 일정규모의 보험적용 병실을 갖추어야 함) <br>※ 다만, 대형병원(상급종합병원)의 1인실 병실료는 건강보험 제외 대상입니다.  <br>▶ 특실, 출산 관련 병실, 정신과병실은 특수성을 감안하여 정보제공에서 제외하였습니다.', 'divCd1Nm': '상급병실료차액', 'divCd2': 'A1300', 'divCd2Dsc': '1개의 입원실에 3인 입원', 'divCd2Nm': '3인실'}, {'divCd1': 'B', 'divCd1Dsc': '초음파를 생성하는 탐촉자를 검사부위에 밀착시켜 초음파를 보낸 다음 되돌아오는 초음파를 실시간 영상화하는 검사입니다.<br><br>※ 아래비용은 진단 목적으로 실시하는 검사로 추적검사는 공개비용에서 제외하였습니다.', 'divCd1Nm': '초음파검사료', 'divCd2': 'B1100', 'divCd2Dsc': '물혹, 염증, 양성종양, 악성종양 등의 진단을 위해 실시하는 검사로 추적검사는 공개비용에서 제외하였습니다.', 'divCd2Nm': '갑상선(부갑상선포함)'}, {'divCd1': 'B', 'divCd1Dsc': '초음파를 생성하는 탐촉자를 검사부위에 밀착시켜 초음파를 보낸 다음 되돌아오는 초음파를 실시간 영상화하는 검사입니다.<br><br>※ 아래비용은 진단 목적으로 실시하는 검사로 추적검사는 공개비용에서 제외하였습니다.', 'divCd1Nm': '초음파검사료', 'divCd2': 'B1200', 'divCd2Dsc': '물혹, 염증, 양성고형종양, 악성종양 등의 진단을 위해 실시하는 검사로 추적검사는 공개비용에서 제외하였습니다', 'divCd2Nm': '유방'}, {'divCd1': 'B', 'divCd1Dsc': '초음파를 생성하는 탐촉자를 검사부위에 밀착시켜 초음파를 보낸 다음 되돌아오는 초음파를 실시간 영상화하는 검사입니다.<br><br>※ 아래비용은 진단 목적으로 실시하는 검사로 추적검사는 공개비용에서 제외하였습니다.', 'divCd1Nm': '초음파검사료', 'divCd2': 'B1300', 'divCd2Dsc': '물혹, 염증, 양성종양, 악성종양 등의 진단을 위해 실시하는 검사로 추적검사는 공개비용에서 제외하였습니다.', 'divCd2Nm': '상복부(간, 담낭, 담도, 비장, 췌장)'}, {'divCd1': 'C', 'divCd1Dsc': '양전자단층촬영(PET)은 양전자를 방출하는 방사성 동위원소(F-18 FDG)를 이용하여 인체의 생화학적, 물리학적 영상을 3차원으로 나타낼 수 있는 검사로, 전산화단층영상진단(CT)과 결합된 PET-CT로 검사하기도 합니다. <br><br>※  아래 비용은 검사에 사용된 의약품과 재료 비용을 포함하므로 병원홈페이지의 고지가격과 상이할 수 있습니다. <br>     아울러, 동 검사료 이외에 영상저장 및 전송시스템료(Full PACS)가 추가로 발생할 수 있습니다.<br>   * "비고"의 조영제는 CT 조영제를 의미.', 'divCd1Nm': 'PET진단료'}, {'divCd1': 'D', 'divCd1Dsc': '캡슐내시경검사는 기존의 내시경검사로 접근이 곤란한 소장부위의 질환을 진단하기 위해 실시합니다. 평평하거나 침윤이 있거나, 염증이 있는 경우에는 기존의 방사선적 검사로 진단하는데 제한이 있어 이를 보완하기 위해 시행합니다.<br><br>※ 아래 비용은 행위료와 재료대를 포함한 비용이므로 병원의 홈페이지 고지가격과 다를 수 있습니다.', 'divCd1Nm': '캡슐내시경검사료'}, {'divCd1': 'E', 'divCd1Dsc': '각종 진단서, 증명서 발급수수료를 말합니다. 일부 진단서는 진찰료가 별도로 발생할 수 있습니다.', 'divCd1Nm': '제증명수수료', 'divCd2': 'E1100', 'divCd2Dsc': '환자의 질병명, 진단날짜, 치료내용, 의사성명 등이 기재<br>※  아래비용은 국문 제증명서 기준입니다.', 'divCd2Nm': '일반진단서'}, {'divCd1': 'E', 'divCd1Dsc': '각종 진단서, 증명서 발급수수료를 말합니다. 일부 진단서는 진찰료가 별도로 발생할 수 있습니다.', 'divCd1Nm': '제증명수수료', 'divCd2': 'E1200', 'divCd2Dsc': '개인의 사망을 증명하는 진단서로 사망원인, 사망일시, 사망장소 등이 기재. 의사가 환자를 진료한지 48시간 내에 예견된 원인으로 사망한 경우 발급 <br>※ 아래비용은 국문 제증명서 기준입니다.', 'divCd2Nm': '사망진단서'}]}
```
># json에서 item 추출

```python
import requests, xmltodict, json

key = "Dc2%2BZbxPbCMQQj%2FNpy%2FQWJXUH7y4MbfPu9HEdJSdVJ3aOXXPQOWIpw0wNWpf%2FV8YAmRSzPhX4U0NodeC1X2%2B3A%3D%3D"
url = "http://apis.data.go.kr/B551182/nonPaymentDamtInfoService/getNonPaymentItemCodeList?pageNo=1&numOfRows=10&ServiceKey={}".format(key)

content = requests.get(url).content
dict = xmltodict.parse(content)
jsonString = json.dumps(dict['response']['body']['items'], ensure_ascii=False)
jsonObj = json.loads(jsonString)

for item in jsonObj['item']:
    print(item['divCd1Nm'])
```
item에서 'divCd1Nm' 항목만을 추출 합니다.

결과
```
상급병실료차액
상급병실료차액
상급병실료차액
초음파검사료
초음파검사료
초음파검사료
PET진단료
캡슐내시경검사료
제증명수수료
제증명수수료
```

# Reference
* https://www.youtube.com/watch?v=iLXNSjjWNtQ&t=1s
* pyCharm 설치 : https://blockdmask.tistory.com/345
