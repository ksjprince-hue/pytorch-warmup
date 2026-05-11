# pytorch-warmup
L-CIS 개발 전 PyTorch 기초 실습


# PyTorch Warmup - 코드 품질 예측기

L-CIS 개발 전 PyTorch 기초 + LLM fine-tuning 실습 프로젝트

## 학습 목표
- PyTorch 기본기 (Tensor, Autograd, nn.Module)
- 딥러닝 학습 파이프라인 직접 구현
- LSTM vs CodeBERT 성능 비교 실험

*** Stage 1 - LSTM (from scratch) ***

### 개념
처음부터 직접 설계하는 모델. 코드를 문자 단위로 토크나이징해서 학습.

### 구조
숫자 시퀀스 → Embedding → LSTM → Linear → 품질 점수

### 결과
- 500 epoch 학습
- 예측값이 평균으로 수렴 (0.82, 0.24 만 반복)
- 개별 코드 특징 학습 실패 → 데이터 부족이 원인

### 한계
데이터 50개로는 처음부터 학습하는 모델에 한계가 있음



*** Stage 2 - CodeBERT fine-tuning ***

### 개념
Microsoft가 GitHub 코드 수백만개로 사전학습한 모델을 가져와서
우리 데이터로 추가 학습 (fine-tuning)

### 구조
코드 입력 → CodeBERT (사전학습) → CLS 토큰 → Linear → 품질 점수

### 결과
- 10 epoch만에 수렴
- 정확도 100% (판정기준 ±0.2)
- 개별 코드마다 다른 점수 예측 성공

## LSTM vs CodeBERT 비교

| | LSTM | CodeBERT |
|---|---|---|
| 학습 방식 | from scratch | fine-tuning |
| epoch | 500 | 10 |
| 결과 | 평균값만 뱉음 | 개별 특징 파악 |
| 정확도 | 100% (but 평균) | 100% (정확) |
| 데이터 의존도 | 높음 | 낮음 |

