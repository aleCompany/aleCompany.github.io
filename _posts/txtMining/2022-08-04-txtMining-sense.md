---
layout: single
title:  "감성분석 - 규칙기반"
categories: txtMining
tag: [감성분석]
toc: true
author_profile: false
---

### 감성분석 이란?
* 문서의 감성/의견/기분 등을 파악하기 위한 기법으로, 편향없는 기계적인 분석
* 분석에는 두가지 방법이 있는데,
  1. 사람이 작성한 규칙 기반 알고리즘을 사용 : 특정 단어와 감성 점수의 쌍을 담은 사전(lexicon) 기반이며, VADER 알고리즘(사이킷런)이 있음
  2. 컴퓨터가 자료로부터 직접 배우는 기계 학습 모형을 사용 : 분류명(레이블)이 붙은 문장 또는 문서 집합을 이용해서 기계 학습 모형을 훈련

### AFINN 활용 : -5~5 사이의 점수 할당 (긍정은 양수, 부정은 음수)
* IMDB 영화 리뷰 데이터를 활용 ( 다운로드 : https://www.kaggle.com/datasets/lakshmi25npathi/imdb-dataset-of-50k-movie-reviews )
<pre>
    import pandas as pd
    import nltk
    from afinn import Afinn
    nltk.download('stopwords')
    from nltk.corpus import stopwords
    from nltk.stem.porter import PorterStemmer
    from nltk.tokenize import RegexpTokenizer
    import numpy as np
    import matplotlib.pyplot as plt

    # 분석파일로드
    file_name = "IMDB Dataset.csv"
    review = pd.read_csv(file_name, engine="python")
    review.head(10)
</pre>
<pre>
    # AFINN 활용 (AFINN: 각 단어 당 -5 ~ 5점으로 구성)
    afinn = Afinn()
    pos_review = review['review'][1] # 하나의 positive 문장만 출력해 봄
    neg_review = review['review'][3] # 하나의 negative 문장만 출력해 봄
    print(afinn.score(pos_review))
    print(afinn.score(neg_review))
    afn = Afinn(emoticons=True)   

    # 첫 n개 문장 극성분석
    n=100
    index = []
    for row in review['review'][0:n]:
        index.append(row)
        print(len(index), 'Predicted Sentiment polarity:', afn.score(row))     
</pre>

### NRC 활용 : 10가지 감정 용어 사전 이용 
* 10가지 감정 : {fear, anger, anticipation, trust, surprise, positive, negative, sadness, disgust, joy}
<pre>
    # NRC 다운로드: https://saifmohammad.com/WebPages/NRC-Emotion-Lexicon.htm
    # 감성 사전로드
    NRC = pd.read_csv(NRC-Emotion-Lexicon-Wordlevelv0.92.txt, engine="python", header=None, sep="\t")

    # 2열이 0 인것은 제외 
    # 예) abacus는 trust라는 감성에 해당됨, abadon은 fear, negative, sadness 3가지에 해당됨.
    NRC = NRC[(NRC != 0).all(1)]

    # 인덱스 번호 리셋
    NRC = NRC.reset_index(drop=True)
    NRC.head(10)
   
    # 특정 한문장 감성분석
    pos_review = review['review'][1] # 하나의 positive 문장만 출력해 봄
    
    tokenizer = RegexpTokenizer('[\w]+')
    stop_words = stopwords.words('english')
    p_stemmer = PorterStemmer()

    raw = pos_review.lower() # 분석하려는 문장
    tokens = tokenizer.tokenize(raw)
    stopped_tokens = [i for i in tokens if not i in stop_words] # 불용어 제거
    match_words = [x for x in stopped_tokens if x in list(NRC[0])] # 사전과 매칭
    
    emotion=[]
    for i in match_words:
        temp = list(NRC.iloc[np.where(NRC[0] == i)[0],1])
        for j in temp:
            emotion.append(j)

    sentiment_result1 = pd.Series(emotion).value_counts()

    #감성으로 분류된 단어 빈도 plot 확인
    print(sentiment_result1, sentiment_result1.plot.bar())    
</pre>

### VADER 어휘사전
* negative, neutral, positive, compound 에 대한 점수 산출
* negative+neutral+positive = 1 이 되도록 구성됨
* compound는 단어의 점수를 합산한 계산되며 -1(극단적 부정) +1(극단적 긍정)으로 Normalize
<pre>
    ! pip install vaderSentiment
    import nltk
    nltk.download('vader_lexicon')
    from nltk.sentiment.vader import SentimentIntensityAnalyzer

    analyser = SentimentIntensityAnalyzer()
    pos_review = review['review'][1] # 하나의 positive 문장만 출력해 봄
    score = analyser.polarity_scores(pos_review) # 극성분석
    print(score) # compund 가 복합적인 종합점수

    # 극성에 따른 분류
    def vader_polarity(text):
        """ Transform the output to a binary 0/1 result """
        score = analyser.polarity_scores(text)
        return 1 if score['pos'] > score['neg'] else 0    

    # 첫 n개 문장만 분석해 보기
    n=10
    index = []

    for row in review['review'][0:n]:
        index.append(row)
        print(len(index), 'Predicted Sentiment polarity:', analyser.polarity_scores(row))
        print(len(index), 'Predicted Sentiment polarity Class:', vader_polarity(row))        
</pre>

### SentiWordNet 어휘사전
* positivity, negativity, objectivity 로 분류
<pre>
    import nltk

    nltk.download('wordnet')
    nltk.download('sentiwordnet')

    from nltk.stem import WordNetLemmatizer
    from nltk.corpus import wordnet as wn
    from nltk.corpus import sentiwordnet as swn
    from nltk import sent_tokenize, word_tokenize, pos_tag

    lemmatizer = WordNetLemmatizer()

    def penn_to_wn(tag):
        """
        Convert between the PennTreebank tags to simple Wordnet tags
        """
        if tag.startswith('J'):
            return wn.ADJ
        elif tag.startswith('N'):
            return wn.NOUN
        elif tag.startswith('R'):
            return wn.ADV
        elif tag.startswith('V'):
            return wn.VERB
        return None    

    def clean_text(text):
        text = text.replace("<br />", " ") # text = text.decode("utf-8")
        return text        

    # 원래는 positivity, negativity, objectivity 3가지 인데, 긍정 부정 2진분류로 변환 시킴
    def swn_polarity(text):
        """
        Return a sentiment polarity: 0 = negative, 1 = positive
        """
        sentiment = 0.0
        tokens_count = 0
        text = clean_text(text)
        raw_sentences = sent_tokenize(text)
        for raw_sentence in raw_sentences:
            tagged_sentence = pos_tag(word_tokenize(raw_sentence))
            for word, tag in tagged_sentence:
                wn_tag = penn_to_wn(tag)
                if wn_tag not in (wn.NOUN, wn.ADJ, wn.ADV):
                    continue
                lemma = lemmatizer.lemmatize(word, pos=wn_tag)
                if not lemma:
                    continue
                synsets = wn.synsets(lemma, pos=wn_tag)
                if not synsets:
                    continue
                synset = synsets[0] # Take the first sense, the most common
                swn_synset = swn.senti_synset(synset.name())
                sentiment += swn_synset.pos_score() - swn_synset.neg_score()
                tokens_count += 1
        if not tokens_count: # judgment call ? Default to positive or negative
            return 0
        if sentiment >= 0: # sum greater than 0 => positive sentiment
            return 1
        return 0 # negative sentiment      

    # 첫 n개 문장만 분석해 보기
    n=10
    index = []

    for row in review['review'][0:n]:
        index.append(row)
        #print(len(index), 'Sentiment:', row['sentiment'])
        print('Predicted Sentiment polarity:', swn_polarity(row))      
</pre>

### TextBlob 활용
* 긍부정 : polatiry [-1 ~ 1]
* 주객관성 : subjectivity [objective:0 ~subjective:1]
<pre>
    !pip install -U textblob
    !python -m textblob.download_corpora
</pre>