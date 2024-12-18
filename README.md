# Jeju_Island_specialty_price_forecasting

# [DACON] 제주도 특산물 가격 예측 AI 경진대회 [(Link)](https://dacon.io/competitions/official/236176/overview/description)

## 🏆 Result
- **최종 3등(🏆)**

<img width="50%" src="https://github.com/jjuhyeok/JEJU-Jeju_Island_specialty_price_forecasting/assets/49608953/1223a8b4-7c3e-4f78-b6c8-e747ab1dfabd"/>  



주최 : 제주특별자치도

주관 : DACON, 제주 테크노파크

규모 : 총 2000여명 참가

===========================================================================


## 🧐 About
제주도 특산물의 가격을 예측하는 AI 모델 개발 및 인사이트 발굴


## [대회 배경]  
제주도에는 다양한 특산물이 존재하며, 

그 중 양배추, 무(월동무), 당근, 브로콜리, 감귤은 제주도의 대표적인 특산물들 중 일부입니다. 

이런 특산물들의 안정적이고 효율적인 수급을 위해서는 해당 특산물들의 가격에 대한 정확한 예측이 필요합니다.

따라서 이번 대회는 특산물 가격 예측에 대한 효율적인 인공지능 알고리즘과 인사이트 발굴이 목표입니다.


## 🔥**대회 접근법**


---

## **Problem Definition**
- **예측 문제:** 단기 예측과 장기 예측의 특성을 반영한 모델링 전략을 사용했습니다.
  - **단기 예측:** 변동성이 높은 단기 가격 예측에는 **딥러닝 시계열 모델**을 활용했습니다.
  - **장기 예측:** 비교적 안정적인 장기 가격 예측에는 **머신러닝 모델**을 사용했습니다.
  - **목표:** 단기와 장기의 예측 특성을 고려해, 두 가지 모델을 결합한 최적의 예측 성능을 도출하는 것입니다.

---

## **Exploratory Data Analysis (EDA)**

### **1. 외부 요인 분석 (EDA 1)**
- **분석 결과:** 재배 면적 증가나 정부의 수급 안정화 정책과 같은 외부 요인들이 가격에 영향을 미칩니다.
- **Feature 생성:** 외부 이벤트(수급 안정 정책, 재배 면적 변화 등)를 고려한 **이벤트 피처**를 추가했습니다.
- **기대 효과:**
  - 외부 요인과 타겟 변수 간의 관계를 학습 가능
  - **정기적인 변동성**과 **특정 이벤트에 따른 변동성**을 구분할 수 있음
  - 유사한 사건이 미래에 발생할 경우 **예측 신뢰성**이 향상됨

### **2. 가격 추세 분석 (EDA 2)**
- **분석 결과:** 가격이 안정적인 경우와 특정 기간 동안 상승/하락하는 **추세 패턴**이 확인되었습니다.
- **Feature 생성:** 장기적인 추세를 반영할 수 있는 **추세 피처**를 추가했습니다.
- **기대 효과:**
  - **단기 변동성**과 **장기 추세** 간의 차이를 명확히 이해하고 반영 가능
  - 모델의 **예측 정확도**와 **일반화 능력**을 향상시킴

### **3. 품목별 차이 분석 (EDA 3)**
- **분석 결과:** 특산물별로 가격 스케일과 추세가 상이합니다.
- **Feature 생성:** 품목별 특성을 반영한 **카테고리 피처**를 추가했습니다.
- **기대 효과:**
  - 품목, 유통 법인 등에 따른 **고유한 시계열 패턴**을 포착 가능
  - 데이터의 **이질성**을 개선하고, 카테고리 간 관계를 발견할 수 있음

---

## **Feature Engineering**

### **1. Trend Features (추세 피처)**
- **Datetime 피처:** 날짜 관련 정보를 활용해, **month, day, weekday** 등의 순환적인 추세를 포착했습니다.
  - **변환 방식:** `sin`, `cos` 변환을 통해 날짜 정보를 정규화하고, `timestamp`를 정수 형식으로 변환해 전체적인 시간 흐름을 반영했습니다.
- **Season 피처:** 특산물의 계절적 패턴을 반영하기 위해, 연중 특정 시기의 계절 변화를 학습하도록 피처를 추가했습니다.
  - **기대 효과:** **계절적 관련성**을 학습함으로써, 과적합을 방지하고 더 넓은 범위에서 계절 패턴을 반영할 수 있음

### **2. Event Features (이벤트 피처)**
- **Holiday 피처:** 가격이 0인 일요일 데이터를 노이즈나 이상치로 인식하지 않도록, **휴일 피처**를 추가했습니다.
- **수확 시기 피처:** 각 특산물의 수확 시기를 반영해, 계절적 패턴과 연중 특정 시기의 가격 변동을 학습할 수 있도록 했습니다.
  - **기대 효과:** 계절적 특성과 수확 시기를 반영해, **과적합을 방지**하고 계절적으로 중요한 패턴에 집중 가능

### **3. Category Features (카테고리 피처)**
- **타겟 인코딩:** 품목별 평균 가격을 계산하여, 각 범주의 **대표적인 가격 수준**을 학습할 수 있도록 했습니다.
- **분산 정규화:** 카테고리 간 타겟값의 분산이 큰 경우, **정규화 기법**을 통해 균형 잡힌 학습을 도모했습니다.
  - **기대 효과:** 카테고리 간의 **다양한 가격 분포**를 반영하고, 균형 잡힌 예측 성능을 확보

---

## **Modeling Approach**

### **1. Time Series Model (모델 1)**
- **시계열 모델:** 각 품목별로 **시계열 모델 (LSTM, ARIMA 등)**을 사용하여 추세를 학습했습니다.
- **모델 튜닝:**
  - **러프한 추세:** 모델을 튜닝하지 않고 기본 설정으로 학습하여 **전체적인 가격 추세**를 반영
  - **타이트한 추세:** 각 품목별로 최적의 하이퍼파라미터를 튜닝하여, **세부적인 변동성**을 더 잘 학습
- **결합 전략:** 러프한 추세와 타이트한 추세를 함께 사용해, 단기와 장기의 가격 변동을 모두 반영할 수 있도록 했습니다.

### **2. Machine Learning Model (모델 2)**
- **머신러닝 모델:** 다양한 Feature Engineering을 통해 **XGBoost, Random Forest** 등 머신러닝 모델을 학습
- **부트스트랩 효과:** 여러 가지 Feature 조합을 활용해, 다양한 데이터를 기반으로 학습하여 **모델의 안정성과 신뢰성**을 높였습니다.
- **주요 Feature:**
  - **Trend Features:** 시간 추세, 계절성, 이벤트 요인
  - **Category Features:** 품목별 평균 가격, 카테고리 간 분산 정규화

### **3. Ensemble Strategy**
- **단기 예측과 장기 예측의 결합:**
  - 시계열 모델의 예측 결과와 머신러닝 모델의 예측 결과를 **앙상블**하여, 단기 변동성과 장기 추세를 동시에 반영
  - 예측 결과의 가중치를 조절하여, 단기 예측 시에는 시계열 모델의 비중을 높이고, 장기 예측 시에는 머신러닝 모델의 비중을 높이는 전략을 사용
- **앙상블 방식:** **Stacking**을 사용해 최종 예측을 산출, 다양한 모델의 예측을 종합하여 성능을 극대화

---

## **Proposed Solutions**

### **Solution 1: 선물 거래 시스템**
- **배경 및 필요성:**
  - 특산물 시장에서 소비자는 가격 변동에 민감하고, 농가 및 유통업체는 재고 관리 및 가격 책정의 어려움이 존재
- **솔루션 제안:**
  - 예측된 가격 정보를 활용한 **선물 거래 시스템** 개발
  - 소비자는 **미리 가격이 낮을 것으로 예상되는 시점**에 농산물을 구매하고, 원하는 시기에 배송받는 서비스
- **기대 효과:**
  - 새로운 서비스 창출로 **농산물 시장의 효율성** 증진
  - 소비자는 **합리적인 가격**에 미래 소비를 계획할 수 있으며, 농가는 **수요 예측**을 통해 생산과 재고 관리의 최적화 가능

### **Solution 2: 맞춤형 보험 상품**
- **배경 및 필요성:**
  - 특산물 시장은 **기후 변화, 수요 변동** 등으로 인해 큰 가격 변동성을 겪음
  - 농가와 유통업체는 이러한 변동성으로 인해 **재정적 리스크**에 직면
- **솔루션 제안:**
  - 예측 모델을 기반으로 한 **맞춤형 보험 상품** 개발
  - 특정 특산물의 가격 변동성에 대해, 특정 기간 동안 낮은 가격 리스크를 평가하고, 이를 바탕으로 **보험료 책정**
- **기대 효과:**
  - 농가 및 유통업체와의 **파트너십 구축** 및 리스크 관리 수단 제공
  - **시장 신뢰성** 강화와 함께, 가격 변동성에 대비한 **재정적 안정성** 제공

---



## 📖 **Dataset Info**

---

### **1. Train Data (파일)**

- **train.csv:**  
  - **기간:** 2019년 01월 01일 ~ 2023년 03월 03일
  - **설명:** 유통된 품목들의 가격 데이터
  - **데이터 필드:**
    - **item:** 품목 코드 (`TG`: 감귤, `BC`: 브로콜리, `RD`: 무, `CR`: 당근, `CB`: 양배추)
    - **corporation:** 유통 법인 코드 (`A` ~ `F` 법인 존재)
    - **location:** 지역 코드 (`J`: 제주도 제주시, `S`: 제주도 서귀포시)
    - **supply (kg):** 유통된 물량 (kg 단위)
    - **price (원/kg):** 유통된 품목의 kg당 가격 (원 단위)

---

### **2. Test Data (파일)**

- **test.csv:**  
  - **기간:** 2023년 03월 04일 ~ 2023년 03월 31일
  - **설명:** 예측 모델의 성능 평가를 위한 테스트 데이터

---

### **3. Submission Format (파일)**

- **sample_submission.csv:**  
  - **설명:** 제출 양식
  - **예측 기간:** 2023년 03월 04일 ~ 2023년 03월 31일
  - **ID:** 품목, 유통 법인, 지역 코드로 구성된 식별자
  - **예측값:** `price (원/kg)` 컬럼에 예측한 가격 기입

---


## 🎈 Modeling
```
**Time-Series**
 - DeepAR

**Machine-Learning**
 - XGBoost
 - CatBoost
 - LightGBM
 

```



  

===========================================================================
  
