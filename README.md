# AI Mathematical Olympiad (AIMO 3)

📢 2026년 겨울학기 [AIKU](https://github.com/AIKU-Official) 활동으로 진행한 프로젝트입니다

## 소개

LLM의 추론 능력은 빠르게 발전하고 있지만, Open source와 Closed source 모델 사이의 성능 차이는 상당합니다. 실제로 지난 AIMO 2에서는 OpenAI가 내부 모델로 50/50 만점을 받은 반면, Open source 최고 점수는 34/50점에 불과했습니다. AIMO는 이러한 기술 격차를 해소하기 위해, Open source LLM을 활용하여 국제 수학 올림피아드(IMO) 수준의 어려운 수학 문제를 해결하는 것을 목표로 하는 대회입니다.

본 프로젝트에서는 LLM이 하나의 문제에 대해 여러 번 풀이를 생성하도록 하고, 생성된 풀이 과정에서 최종 정답 후보를 추출한 뒤, 투표(voting) 및 엔트로피(entropy) 기반 방법을 이용하여 가장 신뢰도가 높은 정답을 선택하는 파이프라인을 설계하였습니다. 본 프로젝트에서는 Open source 모델로 GPT-OSS, DeepSeek, Qwen 등을 사용하였습니다. 또한 엔트로피 방식의 한계를 극복하기 위해, 추론 과정(Chain-of-Thought)을 요약하고 다른 LLM이 이를 검증하는 Verifier 방식 역시 실험해 보았습니다.

## 방법론

본 프로젝트는 다음과 같은 단계로 구성됩니다.

1. **문제 입력**
   - AI Mathematical Olympiad 문제를 입력으로 사용합니다.

2. **풀이 생성 (Solver)**
   - LLM을 이용하여 하나의 문제에 대해 여러 개의 풀이를 생성합니다.
   - 각 풀이에는 문제 해결 과정과 최종 정답이 포함됩니다.

3. **정답 후보 추출**
   - 생성된 풀이 과정에서 최종 정답 후보를 자동으로 추출합니다.

4. **투표 및 엔트로피 기반 선택**
   - 여러 풀이에서 등장한 정답을 기반으로 투표를 수행합니다.
   - 정답 분포의 엔트로피를 고려하여 가장 신뢰도가 높은 후보를 선택합니다.

5. **최종 정답 결정**
   - 투표 결과 및 검증 과정을 기반으로 최종 정답을 선택합니다.

## 환경 설정

본 프로젝트는 Kaggle Notebook 환경에서 노트북으로 진행되었습니다. LLM Inference를 위해 대회에서 NVIDIA H100 GPU를 지원받아 사용했습니다. 따라서 Kaggle 대회 이외의 환경(연구실이 아닌 개인 컴퓨터)에서의 로컬 실행은 어려울 수 있습니다.

## 사용 방법

- 풀 문제를 csv 파일로 준비합니다
- inference_server.run_local_gateway(...)에 파일 경로를 전달합니다
- class CFG에서 debug_trace = True로 설정 시 CoT 로그를 확인할 수 있습니다
- 노트북을 실행하면, 올림피아드 고난도 1문제를 해결하는 기준으로, 약 10분이 소요됩니다
- 대회 제출 + 채점 시에는 약 5시간 이상이 소요됩니다

## 예시 결과

```
Problem:
Let $n \geq 6$ be a positive integer.
We call a positive integer $n$-Norwegian if it has three distinct positive divisors whose sum is equal to $n$.
Let $f(n)$ denote the smallest $n$-Norwegian positive integer.
Let $M=3^{2025!}$ and for a non-negative integer $c$ define 
\begin{equation*}
    g(c)=\frac{1}{2025!}\left\lfloor \frac{2025! f(M+c)}{M}\right\rfloor.
\end{equation*}
We can write 
\begin{equation*}
    g(0)+g(4M)+g(1848374)+g(10162574)+g(265710644)+g(44636594)=\frac{p}{q}
\end{equation*}
where $p$ and $q$ are coprime positive integers. What is the remainder when $p+q$ is divided by $99991$?

Budget: 900.00 seconds | Deadline: 1771434452.45
```
<img width="1137" height="275" alt="image" src="https://github.com/user-attachments/assets/e02ac548-de1b-4138-902e-1728618dd80a" />

## 팀원

[이찬행](https://github.com/Lockway): (Pipeline Engineering)
- Implemented the LLM pipeline
- Implemented entropy voting and experiment code

노윤헌: (Dataset Construction)
- Collected Olympiad-level math problems
- Implemented web scraping for dataset collection

정성윤: (Result Analysis)
- Implemented CoT logging
- Documented experimental findings
