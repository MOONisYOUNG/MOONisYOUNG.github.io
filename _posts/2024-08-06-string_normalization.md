---
layout: single
title: "Unicode 문자열 정규화"
excerpt: "😵‍💫 분명 똑같이 생겼는데 같지 않다고요...? (feat. 날 닮은 너 너 누구야)"
categories:
  - programming-language
tags:
  - Python
  - Trouble Shooting
  - Strings
---
Langchain 라이브러리에 Chat-GPT api를 적용하여 RAG(Retrieval-Augmented Generation) 기술을 구현하다가 기묘한 현상을 목격했습니다.  
특정 조건에 부합하는 한국어 데이터 파일을 Vector Stores에서 꺼내오는 과정 속에서 계속 None 값을 반환하더라구요.  
분명히 해당 파일을 존재하는데 말이지요...!  

이상하다는 생각이 들어서 '파일명의 문자열 값'과 '제가 input으로 넣은 문자열 값'을 비교해 봤습니다.  
그랬더니 아래처럼 서로 다른 값이 나왔습니다.    
분명히 똑같이 생긴 문자열인데 말이지요...?  
```python
file_name_str = "2021노1147"
input_name_str = "2021노1147"

print(file_name_str.encode(encoding='utf-8')) # b'2021\xe1\x84\x82\xe1\x85\xa91147'
print(input_name_str.encode(encoding='utf-8')) # b'2021\xeb\x85\xb81147'
```
  
알고 보니까 utf-8 문자열이어도 정규화 적용 방식에 따라 내부 값이 달라진다고 하네요.  
문자열 정규화 방식 종류들은 아래와 같습니다!
### 👉 Unicode 문자열 정규화 방식
* NFC (Normalization Form C): 완성형으로 변환.
* NFD (Normalization Form D): 조합형으로 변환.
* NFKC (Normalization Form KC): 호환성을 고려한 완성형으로 변환.
* NFKD (Normalization Form KD): 호환성을 고려한 조합형으로 변환.
  
아래와 같이 Unicode 문자열 정규화 과정을 거치니까 두 문자열을 서로 같다고 인식하더라구요.  
참고로 저는 변환 시에 NFC 유형을 사용했습니다~  
```python
import unicodedata as ud
"2021노1147" == ud.normalize('NFC',"2021노1147")  # True
```
  
그리고 제가 사용했던 RAG 코드는 아래처럼 내용을 수정했습니다.  
이렇게 고치니까 원하는 파일을 Vector Stores에서 꺼내올 수 있더라구요.  
```python
import unicodedata as ud

def find_exact_case(case_number):
    for doc in docs:
      doc_source = doc.metadata["source"].split('/')[-1]
      # normalize utf-8 string with NFC type
      doc_source = ud.normalize('NFC', doc_source)
      if doc_source.find(case_number) != -1:
          return doc.page_content
```
  
제가 참고했던 'Unicode 문자열 정규화' 자료도 같이 남깁니다 ^_^  
* <a href="https://withblue.ink/2019/03/11/why-you-need-to-normalize-unicode-strings.html" target="_blank">When "Zoë" !== "Zoë". Or why you need to normalize Unicode strings</a>
* <a href="https://velog.io/@leejh3224/%EB%B2%88%EC%97%AD-%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C-%EC%8A%A4%ED%8A%B8%EB%A7%81%EC%9D%84-%EB%85%B8%EB%A9%80%EB%9D%BC%EC%9D%B4%EC%A7%95-%ED%95%B4%EC%95%BC%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0" target="_blank">[번역] 유니코드 문자열을 정규화 해야하는 이유</a>
* <a href="https://guangyuwu.wordpress.com/2017/09/27/normalizing-unicode/" target="_blank">Python: Normalizing Unicode</a>
