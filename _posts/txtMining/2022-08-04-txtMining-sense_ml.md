---
layout: single
title:  "감성분석 - Machine Learing"
categories: txtMining
tag: [감성분석 - Machine Learing]
toc: true
author_profile: false
---

### IMDB 영화 리뷰 데이터 ML 기반 감성분석( CountVectorizer 활용 )

1 . IMDB 영화 리뷰 데이터를 활용 ( 다운로드 : https://www.kaggle.com/datasets/lakshmi25npathi/imdb-dataset-of-50k-movie-reviews )
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
2 . 데이터 전처리 하기
<pre>
    # HTML 태그 제거 및 데이터 클린닝 를 위한 모듈 설치
    from bs4 import BeautifulSoup
    import re

    # 100 review만 분석
    n = 100 
    reviews = []
    for row in review['review'][0:n]:
        review1 = BeautifulSoup(row, "html5lib").get_text()
        reviews.append(review1)


    # 소대 문자만 추출
    review_list = []
    for row1 in reviews:
        review2 = re.sub('[^a-zA-z]',' ',row1)
        review3 =review2.lower() # 모두 소문자로 변환
        review_list.append(review3)

    # 토큰 처리 
    token_list = []
    for row2 in review_list:
        review4 = row2.split() # 토큰화
        token_list.append(review4)

    # 반복문을 이용하여 불용어 제거
    sentence_words = [w for w in token_list if not w in stopwords.words('english')]

    clean_review = []
    for sentence in sentence_words:
        s = ' '
        clean_review.append(s.join(sentence))
</pre>

3 . DTM, TDM 행렬
   
<pre>   
    # 리뷰의 토큰을 피쳐(독립변수)로 변환
    from sklearn.feature_extraction.text import CountVectorizer
    from sklearn.pipeline import Pipeline

    # 튜토리얼과 다르게 파라미터 값을 수정
    vectorizer = CountVectorizer(analyzer = 'word',
                                tokenizer = None,
                                preprocessor = None,
                                stop_words = None,
                                min_df = 2, # 토큰이 나타날 최소 문서 개수
                                ngram_range=(1, 1), # ngra_range = (최소, 최대)
                                max_features = 20000)

    # 속도 개선을 위해 파이프라인을 사용하도록 개선
    pipeline = Pipeline([('vect', vectorizer),])

    # 벡터화
    data_features = pipeline.fit_transform(clean_review)
    data_features.shape
    data_features.toarray()

    vocab = vectorizer.get_feature_names()

    import pandas as pd
    df = pd.DataFrame(data_features.toarray())
    print(df)
</pre>   

4 . Machine Learing 분석

<pre>  
    # 랜덤포레스트 분류
    from sklearn.ensemble import RandomForestClassifier

    k = data_features.shape[0]
    forest = RandomForestClassifier(n_estimators = 100, n_jobs = -1, random_state=2018)
    forest = forest.fit(data_features, review['sentiment'][0:k])

    # k-fold 검정
    from sklearn.model_selection import cross_val_score

    # k-fold 각각의 ROC AUC 값
    cross_val_score(forest, data_features,review['sentiment'][0:k], cv=10, scoring='roc_auc')

    # k-fold 전체 ROC AUC 평균치
    score = np.mean(cross_val_score(forest, data_features,review['sentiment'][0:k], cv=10, scoring='roc_auc'))
    print(score)
</pre>   

### IMDB 영화 리뷰 데이터 ML 기반 감성분석( TfidfVectorizer 활용 )

1 . TF-IDF 벡터 이용
<pre>
from sklearn.model_selection import train_test_split

# 1000개 리뷰
n = 1000
review = review[0:n]

x_input = review['review']
y_output = review['sentiment']

x_train, x_test, y_train, y_test = train_test_split(x_input, y_output, stratify=y_output, test_size=0.2, random_state=15)

print(x_train.shape, x_test.shape) # 훈련세트, 테스트세트 비율 확인
np.unique(y_train, return_counts=True) # 훈련세트의 타깃(라벨) 확인

from sklearn.feature_extraction.text import TfidfVectorizer

stop_words = stopwords.words('english')

# TF-IDF 가중치를 통해 문서-단어 매트릭스로 바꾸기
vect = TfidfVectorizer(stop_words=stop_words).fit(x_train)
x_train_vectorized = vect.transform(x_train)
</pre>

2 . 로지스틱 회귀 수행
<pre>
from sklearn.linear_model import LogisticRegression, SGDClassifier

model = LogisticRegression()
model.fit(x_train_vectorized, y_train)
print(model.score(x_train_vectorized, y_train))

print(model.score(vect.transform(x_test), y_test))
</pre>

3 . Decision Tree 수행
<pre>
from sklearn.tree import DecisionTreeClassifier

clf = DecisionTreeClassifier()
clf.fit(x_train_vectorized, y_train)

print(clf.score(x_train_vectorized, y_train))
print(clf.score(vect.transform(x_test), y_test))
</pre>