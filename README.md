# 정보보호와 보안 웹공격탐지 프로젝트 11조


[팀원]  
김진수(20141897)[정보보안암호수학과]  
전현근(20153224)[소프트웨어학부]  
장성용(20162838)[소프트웨어학부]  

## 프로젝트 내용

### 프로젝트 개요
- 기본 주어진 머신러닝 모델 코드[base_code.ipynb](https://github.com/jeon-hyeongeun/kookmin_sw_computer_security_project1_group11/blob/main/Base_code.ipynb)를 기반으로 개선된 코드 생성

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

[정상 학습 데이터](https://github.com/jeon-hyeongeun/kookmin_sw_computer_security_project1_group11/blob/main/norm_train.txt)  
[비정상 학습 데이터](https://github.com/jeon-hyeongeun/kookmin_sw_computer_security_project1_group11/blob/main/anomal_train.txt)  
[정상 테스트 데이터](https://github.com/jeon-hyeongeun/kookmin_sw_computer_security_project1_group11/blob/main/norm_test.txt)  
[비정상 테스트 데이터](https://github.com/jeon-hyeongeun/kookmin_sw_computer_security_project1_group11/blob/main/anomal_test.txt)  


### 자료 출처

[CSIC2010](https://www.tic.itefi.csic.es/dataset/)

### 머신러닝 개선 시도 방법
|접근 방법|시도 세부 방법|접근 사유|결과|
|-----|---------------|--------|----|
|벡터 처리 방법|벡터화를 어쩌고 저쩌고|기존 벡터화 방식이 어쩌고 저쩌고|X|

### 향후 보완 요소

### 참고 자료
