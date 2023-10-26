# 불안 데이터 분석 프로젝트

이 프로젝트는 다양한 요인, 예를 들어 나이, 학년, 성별, 육체적 능력(bmi, who_bmi, phq_score)와 불안과의 관계를 이해하는 것을 목표로 합니다.

## 패키지 설치 및 그래프 설정

프로젝트를 시작하기 전, 필요한 R 패키지를 설치하고 그래프 설정을 합니다:
 ```
install.packages(c("tidyverse", "data.table"))
library(tidyverse)
library(data.table)
library(repr)

options(repr.plot.width = 7, repr.plot.height = 7)
```

## 데이터 수집과 정제

이 프로젝트에서 사용할 데이터셋은 다음에서 확인할 수 있습니다:


[데이터 링크](https://www.kaggle.com/datasets/shahzadahmad0402/depression-and-anxiety-data/data?select=depression_anxiety_data.csv)

다음 명령어를 사용하여 데이터를 다운로드하고 확인합니다:

```
# 데이터셋 다운로드
system("gdown --id 1QbSP_vPsddHP4EdKhEFyealseK5b2pyg")

# 데이터셋 로드
DF <- fread("depression_anxiety_data.csv", header = TRUE, encoding = "UTF-8") %>% as_tibble()

# 데이터셋 구조 확인
DF %>% show()

# 데이터셋 구조 확인
DF %>% str()

# 결측값 제거
DF <- na.omit(DF)
table(is.na(DF))

# 이상치 처리 함수
calculate_outliers <- function(data, column_name) {
  # ...
  # 이상치 처리 로직
  # ...
  return(data)
}

# 이상치 처리 적용
DF <- calculate_outliers(DF, "age")
DF <- calculate_outliers(DF, "phq_score")
DF <- calculate_outliers(DF, "bmi")
DF <- calculate_outliers(DF, "gad_score")
DF <- calculate_outliers(DF, "epworth_score")
DF <- na.omit(DF)
boxplot(DF)
```




## 카이제곱 테스트

불안과 다양한 요인 간의 관계를 분석하기 위해 카이제곱 테스트를 수행합니다:

불안과 나이
불안과 학년
불안과 성별
불안과 육체적 능력(who_bmi)

```
install.packages("gmodels")

# 카이제곱 테스트 예시
print("불안과 나이 간의 관계 분석")
gmodels::CrossTable(DF$anxiousness, DF$age, chisq = TRUE, expected = TRUE, prop.r = FALSE, prop.c = FALSE)

# 다른 요인도 이와 유사한 테스트

```

## 로지스틱 회귀

로지스틱 회귀를 사용하여 불안(종속 변수)과 다양한 독립 변수 간의 관계를 분석하였습니다. 그러나 모델은 수렴 문제를 겪었고 최대 반복 횟수를 증가시키거나 모델을 단순화하는 등 다양한 접근 방식을 시도하였으나 문제는 해결되지 않았습니다. 따라 해당 문제를 해결하기 위한 추가 조사가 필요합니다. 


```
print("로지스틱 회귀 모델")
m <- glm(formula = anxiousness ~ ., data = train, family = "binomial")
summary(m)
```

## 문의
프로젝트에 관한 문의나 버그 리포트는 [이슈 페이지](https://github.com/auspicious0/anxiety/issues)를 통해 제출해주세요.

보다 더 자세한 내용을 원하신다면 [보고서](https://github.com/auspicious0/anxiety/blob/main/%EB%A1%9C%EC%A7%80%EC%8A%A4%ED%8B%B1%ED%9A%8C%EA%B7%80_anxiety.ipynb) 를 확인해 주시기 바랍니다.
