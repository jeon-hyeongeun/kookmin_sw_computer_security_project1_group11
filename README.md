# 정보보호와 보안 웹공격탐지 프로젝트 11조


[팀원]  
김진수(20141897)[정보보안암호수학과]  
전현근(20153224)[소프트웨어학부]  
장성용(20162838)[소프트웨어학부]  

## 프로젝트 내용

### 프로젝트 개요
- 기본 주어진 웹공격 탐지 머신러닝 모델 코드<sup>[base_code.ipynb](Base_code.ipynb)<sup>를 기반으로 개선된 코드 생성

### 데이터셋 설명
- 스페인 온라인 쇼핑몰의 HTTP request 로그
- 정상적인 로그들과 비정상 로그가 존재
- 비정상 데이터의 공격 종류
  1. **Static attacks** : 정상적이지 않은 경로로 들어오는 공격들 ex) 존재 하지 않는 리스트에 접근 시도
  2. **Dynamic attacks** : request argument에 대한 공격 ex) sql injection
  3. **Unintentional illegal requests** : 공격 의도를 갖지 않지만 비정상적인 로그를 남김 ex) 전화번호 기입란에 문자를 기입

### 데이터 설명
데이터는 train과 test 데이터로 나누어져 있음
train : test 비율은 8:2

|구분|비정상|정상|
|-----|-----|-----|
|Test|5,013개|7,200개|
|Train|20,052개|28,800개|

[정상 학습 데이터](norm_train.txt)  
[비정상 학습 데이터](anomal_train.txt)  
[정상 테스트 데이터](norm_test.txt)  
[비정상 테스트 데이터](anomal_test.txt)  

데이터 예시
~~~
GET http://localhost:8080/tienda1/publico/anadir.jsp?id=2&nombre=Vino+Rioja&precio=85&cantidad=18&B1=%27+AND+%271%27%3D%271 HTTP/1.1
User-Agent: Mozilla/5.0 (compatible; Konqueror/3.5; Linux) KHTML/3.5.8 (like Gecko)
Pragma: no-cache
Cache-control: no-cache
Accept: text/xml,application/xml,application/xhtml+xml,text/html;q=0.9,text/plain;q=0.8,image/png,*/*;q=0.5
Accept-Encoding: x-gzip, x-deflate, gzip, deflate
Accept-Charset: utf-8, utf-8;q=0.5, *;q=0.5
Accept-Language: en
Host: localhost:8080
Cookie: JSESSIONID=08C8DCD89400C53A4F6F4C6029663578
Connection: close
~~~

### 자료 출처

[CSIC2010](https://www.tic.itefi.csic.es/dataset/)

### 머신러닝 개선 시도 방법
|접근 방법|시도 세부 방법|접근 사유|결과|
|-----|---------------|--------|----|
|벡터 처리 방법|벡터화를 어쩌고 저쩌고|기존 벡터화 방식이 어쩌고 저쩌고|X|

### 향후 보완 요소

### 참고 자료
