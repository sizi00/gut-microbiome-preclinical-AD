# Gut Microbiome Analysis for Preclinical Alzheimer's Disease Early Diagnosis

장내미생물 데이터를 활용해 전임상 알츠하이머(Preclinical AD) 조기 진단 가능성을 분석한 수업 프로젝트입니다.

- **기간:** 2024.09 ~ 2024.12
- **기여도:** 80% (3인 팀)
- **스택:** R (EDA), Python (Random Forest)

---

## Background

알츠하이머병(AD)은 증상이 나타나기 수십 년 전부터 뇌에서 아밀로이드가 축적된다.
전임상 단계에서 조기 진단이 가능하다면 증상 발현 전 치료를 시작할 수 있다.
이 프로젝트는 장내 마이크로바이옴이 비침습적 조기 진단 보조 지표로 활용될 수 있는지 살펴봤다.

---

## Dataset

Ferreiro et al. (2023), *Science Translational Medicine* 논문의 공개 데이터 사용.

- **대상:** 164명 (전임상 AD 49명, 건강 대조군 115명) / 연령 68~94세
- **전임상 AD 정의:** CDR 0이면서 Aβ PET Centiloid > 16.4 또는 CSF Aβ42/Aβ40 < 0.0673
- **데이터 구성:**
  - Meta data: 성별·인종·APOE4·질병 이력·Polygenic Risk Score 등
  - Microbiome: MetaPhlAn3 분류학적 풍부도, HUMAnN3 대사 경로 풍부도

> 논문 및 데이터: https://pmc.ncbi.nlm.nih.gov/articles/PMC10680783/ 의 **Associated Data** 섹션 참고

---

## Analysis Overview

### EDA (R) — Ferreiro et al. 공식 분석 코드 재현
논문에 첨부된 R 분석 코드(`221007_preclinADAnalyses.R`)를 직접 실행·재현하며 분석 파이프라인을 학습했다.

- Alpha / Beta Diversity 비교 (Taxonomic & Pathway)
- PCoA, CAP Ordination + PERMANOVA
- MaAsLin2 음이항 회귀 (AD 연관 미생물 종·대사 경로 탐색)
- Spearman 상관 분석 및 바이오마커 회귀 분석

### ML Modeling (Python) 
`random_forest_model.ipynb` 참고

- **변수 프레임워크:** A(Amyloid), T(Tau), N(Neurodegeneration), G(Genetics), CC(Clinical Covariates)
- **Boruta 알고리즘**으로 유의미한 미생물 taxa 7종 선택
- **8개 모델 설계:** 4가지 변수 조합 × Base / + Taxa
- **실험 방법:** 100 seed 반복 + 10-fold Stratified CV

---

## Results

| Model | Base Acc | + Taxa Acc | Base Sens | + Taxa Sens | Base Spec | + Taxa Spec |
|-------|----------|------------|-----------|-------------|-----------|-------------|
| ALL | 90.8% | 88.5% | 73.7% | 66.9% | 99.9% | 100.0% |
| ALL - A | 71.7% | 71.0% | 24.9% | 21.7% | 96.7% | 97.4% |
| CC + G | 66.3% | **68.5%** | 21.0% | 12.6% | 90.6% | **98.4%** |
| CC | 63.5% | **68.5%** | 17.8% | 12.6% | 87.9% | **98.5%** |

> 각 수치는 100 seed 반복 실험의 평균값

**핵심 결과:**
바이오마커가 부족한 모델일수록 장내미생물 taxa 추가 시 성능 향상폭이 커진다.

- CC 모델: Accuracy +5.0%, Specificity +10.6%
- CC+G 모델: Accuracy +2.2%, Specificity +7.8%

Aβ PET·CSF는 고비용·침습적 검사가 필요하지만, 장내미생물은 분변 샘플만으로 측정 가능하다.
이는 장내미생물이 기존 바이오마커를 보완하는 **비침습적 조기 진단 보조 지표**로서의 가능성을 시사한다.

---

## References

Ferreiro AL, et al. (2023). Gut microbiome composition may be an indicator of preclinical Alzheimer's disease. *Science Translational Medicine*.
https://pmc.ncbi.nlm.nih.gov/articles/PMC10680783/
