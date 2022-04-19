---
layout: single
title:  "사례기반추론 예제 - 1"
categories: casebased
tag: [사례기반추론 예제]
toc: true
author_profile: false
---

간단한 예제를 R을 통하여 알아본다

### 데이터 설명

* 기존데이터 : 기존에 근무하는 직원의 데이터 이다.
  
  |age|job_area|education|income|
  |-|-|-|-|
  |30|accounting|12|50|
  |34|marketing|16|60|
  |47|accounting|18|70|
  |55|accounting|12|55|
  |35|mis|16|58|
  |42|accounting|18|70|
  |47|mis|12|45|

* 신규채용자 : 신규로 들어 오는 직원의 정보인데, 소득을 정하고자 하는 예제 이다.
  |age|job_area|education|income|
  |-|-|-|-|
  |39|mis|14|55|


### R코드 설명
``` {R}

# 거리 정의 및 유사도를 구하는 사용자 정의 함수
# 사용법은 weight와 구하고자 하는 거리인덱스, 원래데이터, 신규데이터 를 정해 주면 됨

dist = function (o_data,n_data, col_name ) { 


    wg = wg_list[[paste('wg', col_name , sep='_')]]
    #등록하지 않는 가중치는 0으로 셋
    if( is.null(wg) ) {  
      wg = 0
    }

    if( class(o_data) == "numeric" || class(o_data) == "integer" ) {
      return ( wg * abs(o_data - n_data)/max(o_data) )
    } else if( class(o_data) == "character" ) {
      return ( ifelse(o_data == n_data, 0,1) )
    } else {
      return ( o_data )
    }
}

cbrUsrFunction = function (oldData, newData , distRange) { 
  
  #거리를 구하지 않는 변수는 맨 마직으로 세팅해야 한다.
  
  dl = list()
  
  #마지막 Target
  for ( k in 1:ncol(oldData) ) {
    
    ret_x = c()
    ret_y = c()
    y_idx = c()
    x_idx = c()    
    
    for( i in 1:nrow(oldData[k]) ) {
      for( j in 1:nrow(newData[k]) ) {
        
          ret_x = c( ret_x, oldData[i,k])  #paste("o",i,"n",j)
          ret_y = c( ret_y, newData[j,k])
          x_idx = c( x_idx, paste("o",i , sep= "") )
          y_idx = c( y_idx, paste("n",j , sep= "") )
      }
    }
    
    #제외변수는 는 거리를 구하지 않음
    if (k %in% distRange ) {    
      ret_dist = dist(ret_x,ret_y, colnames(oldData[k])  )
      dl = cbind(dl, ret_x, ret_y, ret_dist)
      colnames(dl)[which(colnames(dl) == "ret_x")] = paste("x",colnames(oldData[k]))
      colnames(dl)[which(colnames(dl) == "ret_y")] = paste("y",colnames(oldData[k]))    
      colnames(dl)[which(colnames(dl) == "ret_dist")] = paste("diff",colnames(oldData[k]))  
    } else { 
      dl = cbind(dl, ret_x, ret_y)
      colnames(dl)[which(colnames(dl) == "ret_x")] = paste("x",colnames(oldData[k]))
      colnames(dl)[which(colnames(dl) == "ret_y")] = paste("y",colnames(oldData[k]))    
    }
  
  }
  
  df = data.frame(dl)
  
  # numeric,character 등 변환 이 반드시 필요 및 diff 
  # 거리는 모두 numeric
  
  ran = ( distRange ) * 3
  for ( k in ran ) {
    
    if ( class(oldData[,k/3]) == "numeric" ) {
        df[,k-2] = as.numeric( df[,k-2] )      
        df[,k-1] = as.numeric( df[,k-1] )
        df[,k] = as.numeric( df[,k] )
    }
    if ( class(oldData[,k/3]) == "integer" ) {
      df[,k-2] = as.integer( df[,k-2] )      
      df[,k-1] = as.integer( df[,k-1] )
      df[,k] = as.numeric( df[,k] )
    }
    if ( class(oldData[,k/3]) == "character" ) {
      df[,k-2] = as.character( df[,k-2] )      
      df[,k-1] = as.character( df[,k-1] )
      df[,k] = as.numeric( df[,k] )
    }

  }
  
  #마지막컬럼 Target 변경
  if( class( oldData[,ncol(oldData)] ) == "integer") {
    df[,ncol(df)] = as.integer( df[,ncol(df)] )      
    df[,ncol(df)-1] = as.integer( df[,ncol(df)-1] )
  }
  
  
  # Total distance 구하기
  df[,ran] %>% rowSums(na.rm=TRUE) -> df$diff.total
  
  # Simularity 구하기  1 / (1+dist)
  df$Simularity = 1 / (1+df$diff.total)
  
  # Old, new 인덱스 붙이기
  df$x_idx = x_idx 
  df$y_idx = y_idx
  
  df$sel_sim = "-"
  
  # 최고유사선택
  
  for( j in 1:nrow(newData) ) {
    col = paste("n",j , sep= "")
    subdf = df[df$y_idx==col,]
    idx = which.max( subdf[,"Simularity"] )
    yidx = subdf[idx,'y_idx']
    xidx = subdf[idx,'x_idx']
    df[ df$y_idx==yidx & df$x_idx==xidx, 'sel_sim' ] = 'bestSimularity'
    
  }

  return ( df )
}

```

``` {R}
# Case based Reasoning

# 1) Old Case Read
> oCR = read.csv('old_data.csv', header = T , stringsAsFactors=F )
# 2) New Case read
> nCR = read.csv('new_data.csv', header = T , stringsAsFactors=F )
# 3) 가중치 부여 - wg_ 뒤에 헤더이름을 맞추면 됨
> wg_list = list(wg_age=1, wg_job_area=0.15, wg_education=1.5)

# 4) 거리를 구하고자 하는 변수의 순서 결정 ( Cbr에 대한 사용자 함수를 사용하기 위해 )
> getDistVar = c(1,2,3)  #age, job_area, education 3가지 변수로 소득을 결정 하기 위해 컬럼 인덱스 지정

# 5) 거리및 유사도 계산
> df = cbrUsrFunction(oCR, nCR , getDistVar)

# 6) 결과
df[df$sel_sim=='bestSimularity', c(which(colnames(df)=="x.age") , which(colnames(df)=="x.job_area") , which(colnames(df)=="x.education"), which(colnames(df)=="x.income")  , which(colnames(df)=="x_idx") ) ]
```

```
  x.age x.job_area x.education x.income x_idx
5    35        mis          16       58    o5
```

가장 유사한 현재 직원의 경우 5번째 직원으로 소득이 58 수준이다. 따라서 신규 직원도 이를 감안 해서 결정 해야한다고 볼 수 있다.