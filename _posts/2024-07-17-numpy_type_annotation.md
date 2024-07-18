---
layout: single
title: "Numpy 라이브러리 & Python type annotation"
excerpt: "🧐 Numpy 라이브러리를 효과적으로 잘 사용하기 위한 방법 중 하나 "
categories:
  - programming-language
tags:
  - Python
  - Trouble Shooting
---
요즘 Python 언어 사용할 때마다 자료형 표시(type annotation)하는 것에 꽂혔어요.  
Python은 동적 언어라서, 정적 언어를 주로 사용하던 사람들에게 설명할 때마다 소통 비용이 커졌거든요.  
  
이 문제를 어찌 해결하면 좋을지 고민을 좀 하다가, Python type annotation 방식을 알게 되었습니다.  
근데 Python type annotation을 사용하면 기본적인 자료형은 쉽게 표시 가능한데, AI & 데이터 분석에서 자주 쓰는 numpy 라이브러리의 자료형은 표기가 안 되더라구요.  

관련 자료를 찾으면서 StackOverflow 커뮤니티에 저와 같은 고민을 했던 사람들이 존재했단 것을 알게 되었어요.
numpy 자료형 안내 사항을 표기하기 위해서 typing 모듈을 마련했다고 합니다.
이걸 사용하니까 문제가 해결되었어요👍  
  
아무래도 Python 기본 내장형 기능만으로 numpy 자료형 annotation 문제 해결하는 것은 무리였던 것 같습니다...^^;;  
궁금증이 해결되어서 속이 시원하네요!
  
저는 아래와 같은 코드를 작성해서 문제를 해결했습니다 ^_^
  
```python
import numpy as np
import numpy.typing as npt

def typing_np(x1: int, x2: int) -> npt.NDArray[np.int64]:
    result = np.array([x1, x2])
    return result

if __name__ == "main":
    typing_np(1, 2)
```

제가 참고했던 <a href="https://stackoverflow.com/questions/35673895/type-hinting-annotation-pep-484-for-numpy-ndarray" target="_blank">StackOverflow 게시글</a>도 함께 첨부합니다~
