---
layout: single
title:  "텍스트 전처리"
categories: txtMining
tag: [Ai bigData, 텍스트마이닝]
toc: true
author_profile: false
---


### 토큰화

단위에 따르 단어 또는 문자열로 나누는 작업
  * 토큰화 작업시 고려사항
    * 특수문자 구두점 등을 단순하게 제외 하선 안됨
    * 뛰어 쓰기도 하나의 단어로 처리 하는 경우가 있음

  * nltk를 이용해서 토큰화
  <pre>
    import nltk
    nltk.download('punkt')
    from nltk import tokenize
    from nltk.tag import pos_tag

    p = "Apple Inc. is an American multinational technology company that specializes in consumer electronics, software and online services headquartered in Cupertino, California, United States. Apple is the largest technology company by revenue (totaling US$365.8 billion in 2021) and as of June 2022, it is the world's biggest company by market capitalization, the fourth-largest personal computer vendor by unit sales and second-largest mobile phone manufacturer. It is one of the Big Five American information technology companies, alongside Alphabet, Amazon, Meta, and Microsoft"
    print('문장 토큰화 :' , tokenize.sent_tokenize(p))
    print('단어 토큰화 :', tokenize.word_tokenize(p))
    print('품사 태깅 :', pos_tag(tokenize.word_tokenize(p)) )
  </pre>

  * TreebankWordTokenizer 이용해서 토큰화 : 하이픈구성 단어는 하나로 유지
  <pre>
    from nltk.tokenize import TreebankWordTokenizer
    tokenizer=TreebankWordTokenizer()

    p = "Apple Inc. is an American multinational technology company that specializes in consumer electronics, software and online services headquartered in Cupertino, California, United States. Apple is the largest technology company by revenue (totaling US$365.8 billion in 2021) and as of June 2022, it is the world's biggest company by market capitalization, the fourth-largest personal computer vendor by unit sales and second-largest mobile phone manufacturer. It is one of the Big Five American information technology companies, alongside Alphabet, Amazon, Meta, and Microsoft"
    print(tokenizer.tokenize(p))
  </pre>

  * 한글 형태소
    * 한국어 자연어 처리는 KoNLPy(코엔엘파이)라는 파이썬 패키지 사용. 
    * KoNLPy를 통해서 사용할 수 있는 형태소 분석기로 Okt(Open Korea Text), 메캅(Mecab), 코모란(Komoran), 한나눔(Hannanum), 꼬꼬마(Kkma) 등이 있음
    <pre>
      from konlpy.tag import Okt
      from konlpy.tag import Kkma

      okt = Okt()
      kkma = Kkma()
      p = "독립변수를 활용하여 종속변수를 예측 하는 회귀,판별 분석 등이 있음"
      print('OKT 형태소 분석 :',okt.morphs(p))
      print('OKT 품사 태깅 :',okt.pos(p))
      print('OKT 명사 추출 :',okt.nouns(p)) 
    </pre>

### 정규표현식

* 태그 등 필요 없는 글자의 규칙을 제거하는 방법
* 정규식 지원을 위해 re(regular expression) 모듈을 제공하고있음
* 사용법은 
  <pre>
    import re
    p = re.compile("패턴") # 정규식의 객체를 리턴해줌
  </pre>
* 컴파일 옵션 : re.compile("패턴", 옵션)
  <pre>
    re.DOTALL  # 줄 바꿈 문자를 포함하여 모든 문자와 매치
    re.IGNORE  # 대소문자에 관계없이 매치
    re.MULTILINE # 여러 줄과 매치
    re.VERBOSE # 정규식을 보기 편하게 만들 수 있고 주석 등을 사용
  </pre>
* match
  <pre>
    re.compile('패턴').match('문자열').group() # 매치된 문자열
    re.compile('패턴').match('문자열').group(0) # 첫번째 매치된 문자열
    re.compile('패턴').match('문자열').group(n) # n번째 매치된 문자열    
    re.compile('패턴').match('문자열').start() # 매치된 문자열 시작위치
    re.compile('패턴').match('문자열').end() # 매치된 문자열 끝
    re.compile('패턴').match('문자열').span() # 매치된 문자열의 시작, 끝을 리턴
  </pre>

* 패턴 설명
  <table>
    <thead>
      <th>구분</th><th>내용</th>
    </thead>    
    <tbody>
      <tr>
        <td>^</td><td>대괄호의 내용과 반대되는 것을 반환함 ^[0-9]는 숫자를 제외</td>
      </tr>
      <tr>
        <td>$</td><td>종료될 패턴을 의미 ex) abc$ : 123abc 처럼 abc로 종료</td>
      </tr>      
      <tr>      
        <td>+</td><td>앞의패턴이 하나 이상이어야 함 
          <br> ex)<span style="font-Family: Times New Roman">\</span>d+ : 숫자가 최소 1번 이상
          <br>    [a-Z]+ : 문자가 최소 1번 이상
        </td>
      </tr>
      <tr>      
        <td>|</td><td>또는 <br> ex)a|b : a 또는 b 이어야함</td>
      </tr>      
      <tr>      
        <td>?</td><td>앞의 패턴이 없거나 하나이어야 함<br> ex)<span style="font-Family: Times New Roman">\</span>d? : 숫자가 하나 있거나 없어야 함</td>
      </tr>        
      <tr>              
        <td>[문자]</td><td>문자들 중 하나  <br> ex) [a-z] : a-z 중 하나의 문자 </td>
      </tr>
      <tr>              
        <td>[^문자]</td><td>피해야 할 문자들의 집합 <br> ex) [^a-z] : a-z 가 아닌 하나의 문자</td>
      </tr>
      <tr>              
        <td><span style="font-Family: Times New Roman">\</span>d</td><td>숫자0~9</td>
      </tr>      
      <tr>              
        <td><span style="font-Family: Times New Roman">\</span>w</td><td>문자</td>
      </tr>    
      <tr>              
        <td><span style="font-Family: Times New Roman">\</span>s</td><td>whitespace 문자와 매칭,  동일표현 : [ \t\n\r\f\v ] </td>
      </tr>        
      <tr>              
        <td><span style="font-Family: Times New Roman">\</span>S</td><td>whitespace 문자가 아닌 것과 매칭,  동일표현 :  [a-zA-Z0-9_] </td>
      </tr>     
      <tr>              
        <td>dot(.)</td><td>a.b : a와 b 사이의 모든 문자</td>
      </tr>                        
    </tbody>    
  </table>

* 자주사용하는 정규표현식
  <table>
    <thead>
      <th>구분</th><th>내용</th>
    </thead>    
    <tbody>
      <tr>              
        <td>Email 체크</td><td> 
          <pre>
import re
p = re.compile('^[a-zA-Z0-9+-_.]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$')
print(p.match('abc@email.com'))</pre>          
        </td>
      </tr>     
      <tr>              
        <td>숫자만 추출</td><td> 
          <pre>
import re
result = re.sub(r'[^0-9]', '', 'dsaaa1234,ㄹㅇ423fdsaf2.233pp')
print(result)</pre>          
        </td>
      </tr>
      <tr>         
        <td>영문자가 아닌 문자는 공백치환</td><td> 
          <pre>
result = re.sub('[^a-zA-Z]', ' ', '가나 abc TxT 34 마바 Ccc aa')
print(result)</pre>          
        </td>
      </tr>    
      <tr>         
        <td>&lt;Body&gt; ~ &lt;/Body&gt; 내용만 추출</td><td> 
          <pre>body = re.search('<body.*/body>', html, re.I|re.S).group()</pre>          
        </td>
      </tr>     
      <tr>         
        <td>&lt;script&gt; ~ &lt;/script&gt; 내용 삭제</td><td> 
          <pre>re.sub('<script.*?>.*?</script>', '', body, 0, re.I|re.S)</pre>          
        </td>
      </tr>            
      <tr>         
        <td>태그 및 주석 삭제</td><td> 
          <pre>text = re.sub('<.+?>', '', body, 0, re.I|re.S)
print(text)</pre>          
        </td>
      </tr>    
      <tr>         
        <td>\t|\r|\n|\. 제거</td><td> 
          <pre>result = re.sub('\t|\r|\n|\.', '', text)
print(result)</pre>          
        </td>
      </tr>                  
    </tbody>    
  </table>    
### 불용어
  * nltk 불용어 사전
  <pre>
    import nltk
    nltk.download('stopwords')
    stop_words = nltk.corpus.stopwords.words('english')
    len(stop_words)
  </pre>

  * 사이킷런 불용어 사전
  <pre>
    from sklearn.feature_extraction.text import ENGLISH_STOP_WORDS as sklearn_stop_words
    len(sklearn_stop_words)
  </pre>