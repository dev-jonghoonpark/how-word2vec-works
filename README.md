# How word2vec Works

**word2vec — 단어를 벡터로 만드는 법.** “king − man + woman ≈ queen”이 어떻게 가능한지를, 분포 가설에서 출발해 브라우저에서 **직접 임베딩을 학습시키며** 확인하는 인터랙티브 교육 자료입니다.

🔗 **[데모 바로가기](https://dev-jonghoonpark.github.io/how-word2vec-works/)**

## 다루는 내용

1. **왜 벡터인가** · 원-핫의 직교성 문제, 분포 가설(Harris 1954 / Firth 1957)
2. **학습 신호 만들기** · 슬라이딩 윈도우, CBOW vs Skip-gram, 윈도우 크기를 매번 무작위로 뽑는 이유
3. **모델 구조** · 은닉층 없는 행렬 두 개(W, W′), 소프트맥스 수식, NNLM·RNNLM과의 계산량 비교
4. **소프트맥스가 너무 비싸다** · 계층적 소프트맥스(허프만 트리, log₂V번의 이진 결정)와 네거티브 샘플링(k+1번의 참·거짓 판정), 빈도의 3/4 제곱 노이즈 분포
5. **서브샘플링** · P = 1 − √(t/f(w))로 흔한 단어를 버려 속도와 품질을 동시에 얻기
6. **직접 학습시켜 보기** · 브라우저에서 도는 실제 SGNS 구현
7. **벡터 산술과 유추** · 평행사변형 구조, 의미·문법 유추 평가 집합의 구성
8. **구(phrase) 학습과 가법적 구성성** · 동시 등장 점수로 `new york`을 한 토큰으로, 벡터 덧셈이 의미의 AND가 되는 이유
9. **그 뒤에 무슨 일이 있었나** · GloVe, SGNS = PMI 행렬 분해(Levy & Goldberg), 하이퍼파라미터의 비중, fastText, 편향 문제, 문맥 임베딩으로의 전환

## 인터랙티브 데모 9종

| 데모 | 무엇을 확인하나 |
| --- | --- |
| 원-핫 vs 밀집 벡터 | 원-핫은 모든 쌍의 코사인 유사도가 정확히 0 |
| 윈도우로 학습 쌍 만들기 | 중심어·문맥어 하이라이트, Skip-gram/CBOW 전환 |
| 파라미터와 계산량 | 2VN 파라미터, 전체 소프트맥스 vs 계층적 vs 네거티브 (로그 스케일) |
| 허프만 트리와 경로 | 단어 클릭 시 경로 강조, 빈도 가중 평균 경로 vs 균등 트리 |
| 3/4 제곱이 하는 일 | 지수 α에 따른 네거티브 선택 확률 변화 |
| 서브샘플링 | 임계값 t에 따른 단어별 폐기 확률 곡선 |
| **브라우저에서 학습하는 word2vec** | 실제 SGNS 학습 → 주성분 2D 단어 지도, 손실 곡선, 최근접 이웃 |
| 유추 계산기 | 방금 학습한 벡터로 a − b + c, 평행사변형 시각화 |
| 구(phrase) 학습 | 바이그램 점수와 문턱값에 따른 병합 판정 |

여기에 이해도 확인 퀴즈 10문항이 포함되어 있습니다.

### 데모 7은 진짜 학습입니다

애니메이션이 아니라 실제 구현입니다. 윈도우로 쌍을 만들고, 빈도의 3/4 제곱 분포에서 네거티브를 뽑고, 확률적 경사 하강법으로 두 행렬을 갱신합니다. 기본 설정(24차원 · 윈도우 2 · k=6 · 200에폭)에서 몇 초 만에 학습이 끝나며, 결과는 이렇습니다.

- `king`의 최근접 이웃 → `queen`, `throne`, `kingdom`
- `king − man + woman` → **`queen`**
- `prince − boy + girl` → **`princess`**
- 2D 지도에서 남성·여성 단어쌍(father/mother, son/daughter, uncle/aunt …)이 나란히 배치

코퍼스는 직접 고쳐서 다시 학습시킬 수 있습니다.

## 사용 방법

별도 설치 없이 [데모 페이지](https://dev-jonghoonpark.github.io/how-word2vec-works/)를 열면 됩니다. 로컬에서는 `index.html` 파일 하나만 열면 됩니다 — 외부 의존성이 전혀 없습니다.

## 출처와 참고

Tomáš Mikolov 등의 word2vec 논문 두 편이 다루는 개념을 기준으로 본문·그림·데모를 한국어 학습용으로 새로 작성했습니다.

- Mikolov, Chen, Corrado & Dean (2013), *Efficient Estimation of Word Representations in Vector Space* — [arXiv:1301.3781](https://arxiv.org/abs/1301.3781)
- Mikolov, Sutskever, Chen, Corrado & Dean (2013), *Distributed Representations of Words and Phrases and their Compositionality* — [arXiv:1310.4546](https://arxiv.org/abs/1310.4546)

그 밖의 참고 문헌(Harris, Firth, Bengio 등 2003, Goldberg & Levy 2014, Rong 2014, Levy & Goldberg 2014, Pennington 등 2014, Levy 등 2015, Bolukbasi 등 2016, Bojanowski 등 2017)은 페이지 하단에 정리되어 있습니다.

## 관련 자료

[how-ai-works](https://github.com/dev-jonghoonpark/how-ai-works) 시리즈의 일부입니다.

- [How N-gram Works](https://dev-jonghoonpark.github.io/how-n-gram-work/) — 세는 것만으로 언어를 예측하기
- [How RNN Works](https://dev-jonghoonpark.github.io/how-rnn-works/) · [How LSTM Works](https://dev-jonghoonpark.github.io/how-lstm-works/) — 시퀀스 모델
- [How Attention Works](https://dev-jonghoonpark.github.io/how-attention-works/) — 문맥 임베딩으로 가는 길
