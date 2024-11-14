### LLM 애플리케이션의 핵심 단계

LLM애플리케이션은 RAG 파이프라인 구축, 에이전트 구축, 워크플로 구축의 세가지 주요 부분으로 구성되어있고 몇가지 작은 세션이 전후에 있습니다.

- [**LLM 사용**](https://docs.llamaindex.ai/en/stable/understanding/using_llms/using_llms/) : LLM으로 작업을 시작하여 바로 시작하세요.
    - Gemini API 호출을 통해 LLM을 사용하는 방법에 대해 서술합니다.
- **RAG 파이프라인 구축**
    - 검색 증강 생성(RAG)은 데이터를 LLM으로 가져오는 데 중요한 기술이며, 더욱 정교한 에이전트 시스템의 구성 요소입니다.
    - 데이터에 대한 질문에 답할 수 있는 모든 기능을 갖춘 RAG 파이프라인을 구축하는 방법을 서술합니다. 내용은 다음과 같습니다.
        - [**로딩 및 수집**](https://docs.llamaindex.ai/en/stable/understanding/loading/loading/)
            - 비정형 텍스트, PDF, 데이터베이스 또는 다른 애플리케이션에 대한 API 등 데이터가 있는 곳에서 데이터를 가져옵니다.
            - LlamaIndex는 [LlamaHub](https://llamahub.ai/) 에서 모든 데이터 소스에 대한 수백 개의 커넥터를 보유하고 있습니다.
        - [**인덱싱 및 임베딩**](https://docs.llamaindex.ai/en/stable/understanding/indexing/indexing/)
            - 데이터를 얻으면 해당 데이터에 대한 액세스를 구조화하여 애플리케이션이 항상 가장 관련성 있는 데이터로 작동하도록 보장할 수 있는 무한한 방법이 있습니다.
        - [**저장**](https://docs.llamaindex.ai/en/stable/understanding/storing/storing/)
            - LLM에서 제공하는 색인화된 형태나 사전 처리된 요약으로 데이터를 저장하는 것이 더 효율적일 것입니다.
            - 이는 종종 `Vector Store` 라고 알려진 특수 데이터베이스에 저장됩니다. 
            색인, 메타데이터 등을 저장할 수도 있습니다.
        - [**쿼리**](https://docs.llamaindex.ai/en/stable/understanding/querying/querying/)
            - 모든 인덱싱 전략에는 해당 쿼리 전략이 있으며 검색한 내용과 LLM이 반환하기 전에 처리하는 내용의 관련성, 속도 및 정확성을 개선하는 방법에는 API와 같은 구조화된 응답으로 변환하는 것을 포함하여 다양한 방법이 있습니다.
- **에이전트 구축**
    - 에이전트는 도구 세트를 통해 세상과 상호 작용할 수 있는 LLM 기반 knowledge worker 입니다. 이러한 도구는 이전 섹션에서 서술한 RAG 엔진이나 임의의 코드일 수 있습니다.
    - [**기본 에이전트 만들기**](https://docs.llamaindex.ai/en/stable/understanding/agent/basic_agent.md)
        - 도구 세트를 통해 세상과 상호작용할 수 있는 간단한 에이전트를 만드는 방법을 서술합니다.
    - [**에이전트에 RAG 추가**](https://docs.llamaindex.ai/en/stable/understanding/agent/rag_agent/)
        - 이전에 만든 RAG 파이프라인은 에이전트의 도구로 사용할 수 있으며, 에이전트는 이를 통해 강력한 정보 검색 기능을 가질 수 있습니다.
    - [**다른 도구 추가**](https://docs.llamaindex.ai/en/stable/understanding/agent/tools/)
        - API 통합과 같은 더욱 정교한 도구를 에이전트에 추가합니다.
- **워크플로우 구축**
    - 워크플로우는 에이전트 애플리케이션을 구축하기 위한 저수준 이벤트 기반 추상화입니다.
    - 이는 사용자 지정 고급 RAG/에이전트 시스템을 구축하는 데 사용해야 하는 기본 계층입니다.
- [**추적 및 디버깅**](https://docs.llamaindex.ai/en/stable/understanding/tracing_and_debugging/tracing_and_debugging/)
    - **관찰성** 이라고도 하며, LLM 애플리케이션에서 문제를 디버깅하고 개선할 부분을 찾는 데 도움이 되도록 진행 중인 작업의 내부 작동을 살펴보는 것이 특히 중요합니다.
- [**평가**](https://docs.llamaindex.ai/en/stable/understanding/evaluating/evaluating/)
    - 모든 전략에는 장단점이 있으며 애플리케이션을 빌드, 배송 및 발전시키는 데 있어 핵심적인 부분은 변경 사항이 정확성, 성능, 명확성, 비용 등의 측면에서 애플리케이션을 개선했는지 평가하는 것입니다.
    - 변경 사항을 신뢰할 수 있게 평가하는 것은 LLM 애플리케이션 개발의 중요한 부분입니다.