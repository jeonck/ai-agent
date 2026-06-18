---
title: "Eval 프레임워크 및 도구"
weight: 2
---

AI 에이전트와 LLM 서비스를 개발할 때 '감(Vibe Check)'에 의존하지 않고, **수학적·논리적 지표를 기반으로 자동화된 시험대(Eval Pipeline)를 구축할 수 있는 실용적인 도구와 프롬프트 프레임워크**들이 다양하게 존재합니다.

현재 업계에서 가장 많이 활용되는 오픈소스 및 SaaS 도구들을 성격별로 나누어 정리해 드립니다.

---

## 1. 종합 Evaluation 프레임워크 (가장 추천)

에이전트가 통과해야 할 점수판(Metric)을 코드로 작성하고, CI/CD(예: GitHub Actions)에 연동해 테스트할 수 있는 개발자 중심의 도구들입니다.

### [DeepEval](https://deepeval.com/) (오픈소스 / 상용)

현재 에이전트 및 RAG 평가 분야에서 가장 인지도가 높은 파이썬 프레임워크 중 하나입니다. `Pytest`와 네이티브하게 통합되어 개발 프로세스에 녹이기 쉽습니다.

* **장점:** 할루시네이션(환각), 대화 일관성, 보안 위반 검사 등 **50가지 이상의 사전 정의된 메트릭**을 제공합니다. 에이전트가 어떤 도구를 호출했는지(Tool Call Accuracy) 단계를 쪼개어 평가(Span-level scoring)하는 기능이 탁월합니다.
* **특징:** 개발은 로컬에서 무료 오픈소스로 하고, 모니터링은 클라우드 플랫폼인 Confident AI와 연동해 확장할 수 있습니다.

### [Promptfoo](https://github.com/promptfoo/promptfoo) (오픈소스)

CI/CD 파이프라인에 가볍게 붙여 프롬프트와 에이전트 출력을 테스트하기에 최고의 도구입니다. 최근 OpenAI에 인수되며 더욱 주목받고 있습니다.

* **장점:** YAML 파일이나 간단한 스크립트로 테스트 케이스를 수백 개씩 정의해 두고 애플리케이션 변경 시 성능이 떨어지지 않았는지(Regression Test) 빠르게 평가해 줍니다. 보안 취약점을 찔러보는 **레드티밍(Red-Teaming)** 기능이 강력합니다.

### [RAGAS](https://github.com/explodinggradients/ragas) (오픈소스)

에이전트가 외부 지식을 검색하여 답변하는 **RAG(Retrieval-Augmented Generation) 패턴**을 사용할 때 독보적인 도구입니다.

* **장점:** 답변의 신뢰성(Faithfulness), 질문과의 관련성(Answer Relevancy), 문맥 추출 정확도 등 RAG 에이전트의 핵심 성능을 정밀 타격하여 채점해 줍니다. 정답 세트(Ground Truth)가 없어도 AI가 알아서 검증 데이터셋을 합성하고 스스로 평가(LLM-as-a-judge)하는 기능이 우수합니다.

---

## 2. LLM 앱 관측(Observability) 및 실행 추적(Tracing) 도구

에이전트가 다단계(Multi-step)로 사고하고 여러 도구를 복잡하게 호출할 때, 내부에서 무슨 일이 벌어졌는지 눈으로 확인하고 이를 기반으로 Eval을 수행하는 도구입니다.

### [Braintrust](https://www.braintrust.dev/) (상용 SaaS)

에이전틱 워크플로우 평가에 최적화된 엔터프라이즈급 플랫폼입니다.

* **장점:** 에이전트가 실행되는 전 과정의 **추적(Trace) 트리와 로그를 시각적으로 완벽하게 파악**할 수 있으며, 코드 변경에 따른 성능 변화를 대시보드에서 한눈에 보여줍니다. UI가 깔끔하여 기획자나 QA 팀이 협업하여 에이전트를 평가하기 좋습니다.

### [Langfuse](https://langfuse.com/) (오픈소스 / self-hosted 가능)

오픈소스로 제공되는 LLM 엔지니어링 플랫폼으로, 비용 부담 없이 자체 서버에 구축하여 에이전트 실행 과정을 추적하고 채점할 수 있습니다.

* **장점:** LangChain, LlamaIndex 등 주요 에이전트 프레임워크와 쉽게 연동되며, 유저의 피드백 정보나 운영계(Production)에서 발생한 에러 로그를 기반으로 실시간 Eval 점수를 매기기에 유용합니다.

---

## 3. 빅테크 제공 공식 에이전트 Eval 라이브러리

특정 개발 스택이나 에이전트 인프라를 깊게 사용할 때 유용한 공식 도구들입니다.

* **[Agent-EvalKit](https://github.com/awslabs/Agent-EvalKit) (AWS):** AWS 진영에서 공개한 오픈소스 에이전트 평가 툴킷으로, AI 코딩 어시스턴트(Claude Code 등)와 에이전트가 주고받은 궤적을 추적하고 도구 호출 정확도를 체계적으로 검증해 줍니다.
* **[Microsoft.Extensions.AI.Evaluation](https://learn.microsoft.com/en-us/dotnet/ai/evaluation/libraries) (.NET):** 엔터프라이즈 환경에서 C# / .NET 기반으로 AI 에이전트를 개발할 때 마이크로소프트가 공식 지원하는 평가 라이브러리입니다. 작업 준수성(Task Adherence), 인텐트 해결력 등을 측정합니다.

---

## 💡 실용적인 도입 팁

처음 Eval을 도입하신다면, **DeepEval**이나 **Promptfoo** 같은 가벼운 오픈소스 라이브러리를 사용해 내 에이전트 전용 '유닛 테스트 케이스 20~30개'를 먼저 만들어보시는 것을 권장합니다. 코드가 바뀔 때마다 이를 CLI 환경에서 자동으로 돌려보는 것만으로도 품질 관리에 엄청난 대전환을 경험하실 수 있습니다.
