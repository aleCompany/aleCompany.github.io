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

### 불용어 처리

* 불용어 사전 제작 예시
  <pre>
    stop_words = ['a', 'an', 'the', 'on', 'of', 'off', 'this', 'is']
    tokens = ['the', 'house', 'is', 'on', 'fire']
    tokens_without_stopwords =[x for x in tokens if x not in stop_words]
    print(tokens_without_stopwords)
  </pre>

* nltk 불용어 사전 확인 하기
  <pre>
    import nltk
    nltk.download('stopwords')
    stop_words = nltk.corpus.stopwords.words('english')
    len(stop_words)
  </pre>

* nltk을 활용하여 불용어 제거하기
  <pre>
    import nltk
    nltk.download('stopwords')

    from nltk import tokenize


    example = "Apple Inc. is an American multinational technology company that specializes in consumer electronics, software and online services headquartered in Cupertino, California, United States. Apple is the largest technology company by revenue (totaling US$365.8 billion in 2021) and as of June 2022, it is the world's biggest company by market capitalization, the fourth-largest personal computer vendor by unit sales and second-largest mobile phone manufacturer. It is one of the Big Five American information technology companies, alongside Alphabet, Amazon, Meta, and Microsoft"
    stop_words = set(nltk.corpus.stopwords.words('english')) 

    stop_words.append('Microsoft')  # 불용어 단어 목록에 추가

    word_tokens = tokenize.word_tokenize(example)

    result = []
    for word in word_tokens: 
        if word not in stop_words: 
            result.append(word) 

    print('불용어 제거 전 :',word_tokens) 
    print('불용어 제거 후 :',result)
  </pre>

* Gensim을 활용하여 불용어 제거하기
    <pre>
      import gensim

      example = "Apple Inc. is an American multinational technology company that specializes in consumer electronics, software and online services headquartered in Cupertino, California, United States. Apple is the largest technology company by revenue (totaling US$365.8 billion in 2021) and as of June 2022, it is the world's biggest company by market capitalization, the fourth-largest personal computer vendor by unit sales and second-largest mobile phone manufacturer. It is one of the Big Five American information technology companies, alongside Alphabet, Amazon, Meta, and Microsoft"

      filtered_sentence = remove_stopwords(example)

      print('불용어 제거 전 :', list(gensim.utils.tokenize(example, deacc = True)))
      print('불용어 제거 후 :', list(gensim.utils.tokenize(filtered_sentence, deacc = True)))
  </pre>

* Gensim을 활용하여 불용어 추가
    <pre>
      from gensim.parsing.preprocessing import STOPWORDS
      import gensim

      all_stopwords_gensim = STOPWORDS.union(set(['Meta', 'Microsoft']))

      text = "It is one of the Big Five American information technology companies, alongside Alphabet, Amazon, Meta, and Microsoft"
      text_tokens = gensim.utils.tokenize(text)
      tokens_without_sw = [word for word in text_tokens if not word in all_stopwords_gensim]

      print(tokens_without_sw)
  </pre>

* SpaCy 활용하여 불용어 제거하기  
    <pre>
      import spacy

      # 오류 발생시 python -m spacy download en 실행하고 파이참 재실행
      sp = spacy.load('en_core_web_sm') 

      all_stopwords = sp.Defaults.stop_words
      all_stopwords.add("Microsoft")

      example = "Apple Inc. is an American multinational technology company that specializes in consumer electronics, software and online services headquartered in Cupertino, California, United States. Apple is the largest technology company by revenue (totaling US$365.8 billion in 2021) and as of June 2022, it is the world's biggest company by market capitalization, the fourth-largest personal computer vendor by unit sales and second-largest mobile phone manufacturer. It is one of the Big Five American information technology companies, alongside Alphabet, Amazon, Meta, and Microsoft"

      text_tokens = word_tokenize(example)
      tokens_without_sw = [word for word in text_tokens if not word in all_stopwords]

      print(tokens_without_sw)      
  </pre>

* 사이킷런 불용어 제거하기
  <pre>
    from sklearn.feature_extraction.text import ENGLISH_STOP_WORDS as sklearn_stop_words
    from nltk.tokenize import word_tokenize

    example = "Apple Inc. is an American multinational technology company that specializes in consumer electronics, software and online services headquartered in Cupertino, California, United States. Apple is the largest technology company by revenue (totaling US$365.8 billion in 2021) and as of June 2022, it is the world's biggest company by market capitalization, the fourth-largest personal computer vendor by unit sales and second-largest mobile phone manufacturer. It is one of the Big Five American information technology companies, alongside Alphabet, Amazon, Meta, and Microsoft"

    word_tokens = word_tokenize(example)
    result = []

    for word in word_tokens: 
        if word not in sklearn_stop_words: 
            result.append(word) 

    print('불용어 제거 전 :',word_tokens) 
    print('불용어 제거 후 :',result)
  </pre>

* 한국어 불용어 제거하기
  * 한국어는 조사, 접속사를 토큰화 단계에서 제거 함으로 별도의 불용어사전이 없지만, 사용자가 정의해서 사용하면 됨
  <pre>
    from nltk.tokenize import word_tokenize 

    example = "형용사 제거하고 싶으면 제거 하고 그렇지 않으면 그냥 사용 하시면 됩니다."

    word_tokens = word_tokenize(example)
    stop_words= ['형용사' , '제거']

    result = [] 
    for w in word_tokens: 
        if w not in stop_words: 
            result.append(w) 

    print('불용어 제거 전 :',word_tokens) 
    print('불용어 제거 후 :',result)
  </pre>
  
### 어간추출(stemming)
단어의 어미를 자르는 작업을 의미 Ex) goodness -> good , allowance -> allow 등

* 말미의 s 제거 예제
<pre>
  import re
  def stem(phrase):
      return ' '.join([re.findall('^(.*ss|.*?)(s)?$', word)[0][0].strip("'") for word in phrase.lower().split()])
  print (stem('His whose kbs'))
</pre>


* 영어 어간 추출 예제
<pre>
  from nltk.stem.porter import PorterStemmer
  stemmer = PorterStemmer()
  ' '.join([stemmer.stem(w).strip("") for w in "dish washer's washed dished. data mining, miners, mines".split()])
  stemmer.stem('goodness')
</pre>

<pre>
  import nltk
  stem = nltk.stem.SnowballStemmer('english')
  stem.stem("dish washer's washed dished. data miners, mining")
</pre>

### 텍스트 전처리 예시

1. 종합예제
  
<pre>
  import re
  import nltk
  nltk.download('stopwords')
  from nltk.corpus import stopwords
  from nltk.stem import PorterStemmer
  ps = PorterStemmer()

  raw_review = "Okay let's be honest I can't rate this product on accuracy or battery life since I only have had the watch 3 days. So the thickness is 5 star feels good on wrist, 5 star on yes it is an Active 2 watch. </br> On it pairing ok it does great on my OnePlus 8  phone and as of right now all works great. In 1 month I will come back to review area and give the 2 important parts review. UPdate: Okay had the watch a little over a month and I must say it all works great the watch battery life is 1and a half days with reminders and gym workouts so over all its a great watch that works"

  # 1. HTML 제거
  review_text = BeautifulSoup(raw_review, 'html.parser').get_text()
  # 2. 영문자가 아닌 문자는 공백으로 변환
  letters_only = re.sub('[^a-zA-Z]', ' ', review_text)
  # 3. 소문자 변환
  words = letters_only.lower().split()
  # 4. Stopwords를 세트로 변환 (리스트보다 set이 빠름)
  stops = set(stopwords.words('english'))
  # 5. Stopwords 제거
  meaningful_words = [w for w in words if not w in stops]
  # 6. 어간추출
  stemming_words = [ps.stem(w) for w in meaningful_words]
  # 7. 공백으로 구분된 문자열로 결합하여 결과를 반환
  print(' '.join(stemming_words))
</pre>  
