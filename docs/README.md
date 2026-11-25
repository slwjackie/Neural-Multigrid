# 문서 안내

브랜치: **`cnn-prolongation-bamg-energy-teacher`**. 현재 진입 경로는 **BAMG + energy teacher → λ=10·30·50별 Weight CNN 학습 → validation 기반 격자별 선택 → freeze → final/OOD**다. 최신 추가 기능은 `row9`·`symmetric9`의 9채널 행렬 특징이며, 기존 10채널 경로도 비교·재현용으로 유지한다.

새 사용자는 [영문 README](../README.md) 또는 [국문 README](../README_KR.md)에서 시작한다. 저장소의 기본 연구 설정은 특징 모드·데이터 규모마다 **scratch/warm × λ 3개 × seed 3개 = 18개 학습**이다. 로컬 scratch 전용 수정본을 기본 설정으로 설명하지 않는다.

## 현재 실험 문서

| 문서 | 내용 |
| --- | --- |
| [bamg_feature9.md](bamg_feature9.md) | 최신 `row9`/`symmetric9` 특징, 실제 입력 28채널, warm 변환, 동일 development cohort, 전용 실행기 |
| [bamg_energy_cnn_grid_routing.md](bamg_energy_cnn_grid_routing.md) | λ=10·30·50, 18개 학습, 560/1,120 TRAIN, 280/56 validation, 격자별·전역 λ 선택, final/OOD |
| [bamg_cnn_low_memory_recovery.md](bamg_cnn_low_memory_recovery.md) | 기존 10채널 routing의 label 메모리 부족 복구, 한 case씩 로딩, 실행 provenance와 재개 |
| [bamg_source_freeze_compatibility.md](bamg_source_freeze_compatibility.md) | 기존 source freeze digest 호환성, 보존해야 할 source/teacher/run 해시 |
| [bamg_energy_cnn_initialization_comparison.md](bamg_energy_cnn_initialization_comparison.md) | 단일 선택 λ의 scratch/warm 실험, epoch shuffle, 층화 데이터, 공통 1,270개 holdout 설계 |
| [bamg_energy_teacher_comparison.md](bamg_energy_teacher_comparison.md) | CNN 없는 수치 teacher 비교, 기본 λ 0·0.01·0.1·1·5·10·30·50, 정규화 목적함수, 과거 pilot 구분 |
| [bamg_compact_weight_study.md](bamg_compact_weight_study.md) | Fixed bilinear support Weight CNN source 경로, warm initialization, 학습·검증 |
| [bamg_two_cnn_study.md](bamg_two_cnn_study.md) | BAMG teacher 생성과 CNN 비교군 |
| [Q1_FEM_KR.md](Q1_FEM_KR.md) | Q1 요소, 경계, quadrature, discrete manufactured RHS와 FEM load 구분 |

## 명령의 범위

- `run`은 라벨 생성·학습·development validation·격자별 선택까지만 수행한다.
- `freeze`는 완료된 학습과 선택 근거를 검증하고 모델·규칙·데이터·출처를 고정한다.
- `evaluate-final`이 final과 모든 OOD를 평가한다. 처음에는 `--resume` 없이 시작하고, 중단 후 같은 평가를 재개할 때 붙인다.
- `report`와 `report-final`은 저장된 결과를 읽는다. 학습이나 평가를 새로 수행하지 않는다.

각 output에는 처음 선택한 실행기를 계속 사용한다. Feature9는 자체 label 메모리 절약 경로를 포함한다. 기본 routing 문서의 원래 실행기와 저메모리 adapter의 차이는 저메모리 문서에서 확인한다. 데이터 규모·특징 모드·학습 예산은 final/OOD를 열기 전에 development validation으로 결정한다.
