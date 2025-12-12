# MEMIT with Model Merging Framework

다중 지식 편집에서 모델 병합을 통한 성능 개선 연구

## 🚀 Quick Start

### 1. 독립적 모델 편집 (MEMIT 환경)

여러 규모의 지식을 독립적으로 편집하여 모델을 저장합니다.

```bash
python -u -m falcon.tester --identical_nums 1 1 1 1 1 1 1 1 1 --num_edits_list 20 50 100 250 500 1000 3000 5000 10000
```

**결과**: `edited_models/` 폴더에 편집된 모델들이 저장됩니다.
- `edited_1_20_1/`, `edited_1_50_1/`, ..., `edited_1_10000_1/`

### 2. 모델 병합 (Mergekit 환경)

편집된 모델들을 다양한 병합 방법으로 결합합니다.

#### 기본 병합 예시
```bash
python merge_runner.py --model_dirs edited_2_1500_1 edited_2_1500_2 --merge_methods ties --lambdas 1.6 --densities 0.5
```

#### 다양한 하이퍼파라미터 탐색
```bash
python merge_runner.py --model_dirs edited_2_25_1 edited_2_25_2 --merge_methods task_arithmetic ties dare_ties dare_linear della della_linear --lambdas 1.1 1.3 1.5 --densities 0.1 0.3 0.5 0.7 0.9 --epsilons 0.1 0.2 0.3 0.4
```

**결과**: `merged/` 폴더에 병합된 모델들이 저장됩니다.
- `edited215001_edited215002_ties_l1.6_d0.5/`

### 3. 성능 평가

편집 및 병합된 모델의 성능을 측정합니다.

```bash
python -u -m falcon.tester --identical_nums 1 --num_edits_list 10000 --test_type all
```

**결과**: `results/MEMIT/` 폴더에 평가 결과가 저장됩니다.

### 4. 결과 분석

전체 실험 결과를 요약하고 시각화합니다.

```bash
# 기본 요약
python -m experiments.summarize --dir_name MEMIT --runs all

# 상세 분석
python all_summary.py --detailed --analysis

# 시각화 포함
python all_summary.py --detailed --analysis --plot --plot_output "result.png"
```

**결과**: 
- 터미널에 성능 메트릭 테이블 출력
- `result.png` 시각화 파일 생성

## 📁 주요 폴더 구조

```
memit/
├── edited_models/     # 편집된 모델들
├── merged/           # 병합된 모델들  
├── results/          # 평가 결과
├── logs/            # 실행 로그
└── data/            # 데이터셋
```

## 지원하는 병합 방법

- `task_arithmetic`: 기본 태스크 산술
- `ties`: TIES 방법
- `dare_ties`: DARE-TIES 방법  
- `dare_linear`: DARE Linear 방법
- `della`: DELLA 방법
- `della_linear`: DELLA Linear 방법

## 평가 메트릭

- **Efficacy**: 편집 효과성
- **Generalization**: 일반화 성능
- **Specificity**: 특이성
- **Fluency**: 유창성
- **Consistency**: 일관성
