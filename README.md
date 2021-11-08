# 정보보호와 보안 웹공격탐지 프로젝트 11조


[팀원]  
김진수(20141897)[정보보안암호수학과]  
전현근(20153224)[소프트웨어학부]  
장성용(20162838)[소프트웨어학부]  

## 프로젝트 내용

### 프로젝트 개요
- 기본 주어진 웹공격 탐지 머신러닝 모델 코드<sup>[base_code.ipynb](Base_code.ipynb)</sup>를 기반으로 개선된 코드 생성


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

1. 접근 방법 : 머신러닝 모델 변경
  - 시도 세부 방법 : 다른 이분분류를 사용하는 머신러닝 모델 3가지를 테스트 하여 비교<sub>[커널서퍼포트벡터머신/KNN/의사결정나무]</sub>
  - 코드 예시
```python
def train(train_vec,train_y): #Kernelized Support Machine
  svm = SVC(C=100)
  svm.fit(train_vec, train_y)
  return svm
```

2. 접근 방법 : 벡터 처리 방식 변경
  - 시도 세부 방법 : 임베딩(벡터화)하는 방법으로는 기존에 윈-핫 인코딩을 사용하지만, 데이터 간 연관 관계를 표현할 수 없다는 단점이 있어 이를 보완한 인베딩 기법인 doc2vec으로 벡터화 변경 시도
  - 코드 예시
```python
def vectorize(train_x, test_x):
  model = Doc2vec(data, vector_siez=5, window=3, min_count=1, workers=4)
  inferred = model.infer_vector(['GET', 'POST', 'http'])
  train_vec =inferred(train_x)
  test_vec = inferred(test_x)
  return train_vec, test_vec
```

3. 접근 방법 : randomforestclasssfier의 최적 파라미터 검색
  - GridSearchCV를 통해 최적 파라미터를 검색
  - 코드 예시

```python
def train(train_vec, train_y):
  rf = randomForestClassfier()
  
  params = {'n_estimators' : [10, 100],
            'max_depth' : [6, 8, 10, 12],
            'min_samples_leaf' : [8, 10, 12],
            'min_samples_split' : [8, 16, 20]
            }
  grid_cv = GridSearchCV(rf, param_grid = params, cv = 3, n_jobs = -1)
  grid_cv.fit(train_vec, train_y)
  
  print('촤적 파라미터: ', grid_cv,best_params_)
```

4. 접근 방법 : 파싱 데이터 정제
  - 비정상 적인 웹공격 탐지를 하기 위해 필요한 method와 body만 파싱해 데이터를 분석
  - 코드 예시

```python
def parsing(path):#파싱을 진행하는 함수
    with open(path,'r',encoding='utf-8') as f:#파일을 읽어드리고 ['로그','로그',...] 이런식으로 
        train=[]
        para=""
        while True:
            l = f.readline() #한줄씩 읽어 옵니다
            if not l:
                break #파일을 전부 읽으면 읽기를 중단합니다.
            if l != "\n":
                if l[:3] == 'GET' or l[:4] == 'POST': #method가 있는 라인만 받아옵니다.
                  para +=l
            else:
                if para!='':
                    if para[:4]=='POST': #Method가 POST인 경우 예외적으로 바디까지 가져옵니다.
                        para+=f.readline()
                    train.append(para)
                    para=""
    return train
```

### 결론

acuurancy score와 f1 score의 결과값이 가장 유의미한 파싱 데이터 처리 방식 변경을 채택<sup>[Submit_code.ipynb](Submit_code.ipynb)</sup>

### 참고 자료

[한국정보통신학회논문](https://www.koreascience.or.kr/article/JAKO201905653788969.pdf) - 벡터화 처리 방식 변경 시도
