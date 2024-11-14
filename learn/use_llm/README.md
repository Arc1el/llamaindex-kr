LLM 애플리케이션을 생성할때 가장 첫번째로 진행해야 할 단계는 어떤 LLM을 사용할것인가 입니다. 그리고 이때 원한다면 두개 이상의 LLM을 사용할 수 있으며 이는 사용자의 선택입니다.

LLM은 여러 단계에서 사용될 수 있습니다.

- 인덱싱
    - 인덱싱하는동안 데이터의 관련성 (예를 들어 인덱싱을 할지 여부)을 확인 할 수 있음.
    - LLM을 사용하여 로우 데이터를 요약하고 요약내용을 인덱싱.
    - 기타 외 모든것
- LLM 쿼리
    - 검색 (인덱스에서 데이터 가져오는것)중 다양한 옵션 (다른 인덱스)을 통해 원하는 정보를 찾을 수 있는 최적의 위치 (인덱스)에 대한 결정을 내릴 수도 있습니다.
        - 이때 에이전트로 구성된 LLM은 Tool(도구)를 사용하여 원하는 데이터 소스에 접근할 수 있습니다.
    - 응답 합성 (검색된 데이터를 답변으로 변환합니다.)중에 여러 쿼리에 대한 답변을 하나의 쿼리로 결합하거나 unstructed 데이터들에 대해 json이나 다른 프로그래밍 출력 형식으로 변환할 수도 있습니다.

라마인덱스는 다양한 LLM들에 대한 단일 인터페이스를 제공해 다음과 같이 간단하게 구현할 수 있습니다. 본 문서에서는 Google의 Gemini-Flash 를 사용하여 진행해보도록 하겠습니다.

```python
from llama_index.llms.gemini import Gemini

response = Gemini(model_name="models/gemini-1.5-flash-latest").complete("Paul Graham은 누구인가요?")
print(response)
```

> 폴 그레이엄은 미국의 컴퓨터 과학자, 사업가, 작가입니다. 그는 Y Combinator의 공동 설립자이자 회장이며, 에세이와 블로그 게시물을 통해 컴퓨터 과학, 스타트업, 문화에 대한 통찰력을 공유하는 것으로 유명합니다.
> 
> 
> 그레이엄은 1964년에 미국 펜실베이니아주 필라델피아에서 태어났습니다. 그는 하버드 대학교에서 수학과 컴퓨터 과학을 전공했으며, 1985년에 졸업했습니다. 졸업 후 그는 컴퓨터 과학 분야에서 일했으며, 1990년대 초반에는 Lisp 프로그래밍 언어에 대한 연구를 수행했습니다.
> 
> 1990년대 후반, 그레이엄은 Robert Morris와 함께 Viaweb이라는 회사를 설립했습니다. Viaweb은 온라인 상점을 위한 소프트웨어를 개발했으며, 1998년에 Yahoo!에 인수되었습니다. 그레이엄은 Yahoo!에서 몇 년 동안 일했으며, 2005년에 Y Combinator를 공동 설립했습니다.
> 
> Y Combinator는 스타트업을 위한 투자 회사이자 인큐베이터입니다. 그레이엄은 Y Combinator를 통해 수많은 성공적인 스타트업을 지원했으며, 그 중에는 Airbnb, Dropbox, Stripe 등이 있습니다.
> 
> 그레이엄은 또한 에세이와 블로그 게시물을 통해 컴퓨터 과학, 스타트업, 문화에 대한 통찰력을 공유하는 것으로 유명합니다. 그의 글은 종종 논쟁적이지만, 항상 생각을 자극하는 것으로 알려져 있습니다.
> 
> 그레이엄은 컴퓨터 과학 분야에 대한 그의 공헌과 스타트업 생태계에 대한 그의 영향으로 널리 알려져 있습니다. 그는 또한 그의 글을 통해 컴퓨터 과학과 기술에 대한 대중의 이해를 높이는 데 기여했습니다.
> 

일반적으로는 다음과 같이 LLM을 인스턴스화 하여 Settings를 설정한 다음 진행합니다.

이경우 Google의 Gemini-flash 모델을 인스턴스화 하고 사용하도록 지정하였고, 벡터스토어 구성시 사용될 임베딩 모델에 대해서도 한국어 멀티모달 모델을 지정하였습니다.

```python
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader, Settings
from llama_index.embeddings.huggingface import HuggingFaceEmbedding
from llama_index.llms.gemini import Gemini

Settings.embed_model = HuggingFaceEmbedding(model_name="nlpai-lab/KoE5")
Settings.llm = Gemini(model_name="models/gemini-1.5-flash-latest")

documents = SimpleDirectoryReader(input_dir="data").load_data()

index = VectorStoreIndex.from_documents(documents)

query_engine = index.as_query_engine()
response = query_engine.query("Paul Graham은 누구인가요?")

print(response)
```

> Paul Graham은 컴퓨터 과학자이자 작가입니다.
>