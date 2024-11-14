## 대규모 언어 모델 (LLMs)이란?

LlamaIndex가 나오게 된 핵심 기술입니다. 쉽게 말하면 우리가 쓰는 언어를 이해하고, 새로운 글도 만들 수 있는 AI 컴퓨터입니다. 학습한 내용이나 우리가 새로 알려준 정보를 가지고 질문에 답변할 수 있습니다.

## 검색 증강 생성 (RAG)이란?

LlamaIndex로 데이터 기반 LLM 앱을 만들 때 꼭 필요한 기술입니다. 재미있는 점은 LLM을 다시 학습시키지 않아도 우리만의 비공개 데이터로 질문-답변이 가능하다는 것입니다. 모든 데이터를 한 번에 보내지 않고, 필요한 부분만 골라서 보내니까 효율적입니다!

## 에이전트란?

LLM과 다른 도구들을 잘 섞어서 반자동으로 일을 처리하는 소프트웨어입니다. 혼자서도 꽤 많은 일을 할 수 있습니다.

## 주요 활용 사례

### 1. 데이터 추출하기

- Pydantic이라는 도구로 원하는 형태의 데이터를 뽑아낼 수 있습니다
- PDF나 웹사이트 같은 정리되지 않은 자료에서 필요한 정보만 추출합니다
- 덕분에 업무 자동화가 훨씬 쉬워집니다

### 2. 질문하고 답변받기

- 데이터에 대해 궁금한 점을 물어보면 답을 찾아줍니다
- 일상적인 말로 물어봐도 알아듣고, 근거자료와 함께 답변해줍니다

### 3. 대화하기

- 한 번의 질문-답변으로 끝나지 않고 계속 대화를 이어갈 수 있습니다
- 마치 실제 대화하는 것처럼 자연스럽습니다

### 4. 자동으로 일처리하기

- LLM이 스스로 판단해서 일을 처리합니다
- 여러 도구들을 능숙하게 사용할 수 있습니다
- 정해진 순서 없이도 상황에 맞게 일을 처리할 수 있습니다
- 복잡한 일도 유연하게 해결할 수 있습니다

## 스타터 튜토리얼

### 설치 및 설정

LlamaIndex 생태계는 네임스페이스 패키지 컬렉션을 사용하여 구성됩니다.

이는 사용자에게 LlamaIndex에 핵심적인 스타터 번들이 제공되며 필요에 따라 추가 통합을 설치할 수 있음을 의미합니다.

패키지와 사용 가능한 통합의 전체 목록은 [LlamaHub](https://llamahub.ai/) 에서 확인할 수 있습니다 .

해당 도큐먼트에서는 Google의 Gemini-Flash 모델을 사용하여 특별한 과금 없이 진행할 수 있도록 하겠습니다.

### pip을 사용한 빠른 설치

다음 pip 명령어를 사용하여 llamaindex를 설치할 수 있습니다. 이때 google의 gemini를 사용하기 위한 번들도 함께 설치하도록 하겠습니다.

```jsx
pip install llama-index llama-index-multi-modal-llms-gemini
```

gemini를 사용하기 위한 번들에 대한 정보는 다음에서 확인할 수 있습니다.

https://llamahub.ai/l/multi_modal_llms/llama-index-multi-modal-llms-gemini?from=

### Google Generative AI 환경설정

Google의 Gemini (제미나이)를 사용하기 위해 api 키를 환경변수로 설정해야합니다.

```python
# 환경 변수로 설정
export GOOGLE_API_KEY=your_api_key_here

# 쥬피터 환경의 매직커맨드로 설정
%env GOOGLE_API_KEY=your_api_key_here

혹은 .env등을 활용하여 "GOOGLE_API_KEY" 를 설정할 수 있도록 합니다.
```

### 데이터 로드 및 인덱스 구축

다음 데이터를 활용하여 다음과 같은 디렉토리 구조로 설정할 수 있도록 합니다.

```
├── starter.py
└── data
    └── paul_graham_essay.txt
```

### 데이터 로드 및 인덱스 구축

data폴더와 동일한 경로에서, 다음을 진행합니다.

```python
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader, Settings
from llama_index.embeddings.huggingface import HuggingFaceEmbedding
from llama_index.llms.gemini import Gemini

Settings.embed_model = HuggingFaceEmbedding(model_name="nlpai-lab/KoE5")
Settings.llm = Gemini(model_name="models/gemini-1.5-flash-latest")
```

> **갑자기 임베드 모델은 왜 사용되었나요?**
라마인덱스 구축시 기본적으로 OpenAI의 임베딩 모델을 사용하게 되는데, 우리는 zero cost가 목적이므로 로컬에서 사용할 수 있는 허깅페이스 임베딩 모델을 사용하도록 하겠습니다.
> 

### 데이터 쿼리

Q&A를 위한 간단한 엔진을 생성합니다. 이 생성된 엔진을 활용하면 답변을 전달받을 수 있습니다.

```python
query_engine = index.as_query_engine()
response = query_engine.query("저자는 어렸을때 어떤일을 했나요? 자세히 알려주세요.")
print(response)
```

> 저자는 어렸을 때 글쓰기와 프로그래밍을 했습니다. 특히 단편 소설을 썼지만, 그 당시에는 별로 좋지 않았다고 합니다. 또한 학교에서 사용하는 IBM 1401 컴퓨터를 이용하여 프로그래밍을 시작했습니다. 하지만 당시에는 컴퓨터 사용이 제한적이었고, 저자는 컴퓨터를 제대로 활용하지 못했습니다.
> 

### 로깅을 활용한 쿼리와 이벤트 확인하기

로깅 라이브러리를 사용하면 실제로 LLM이 어떻게 추론되는지를 확인할 수 있습니다.

```python
import logging
import sys

logging.basicConfig(stream=sys.stdout, level=logging.DEBUG)
logging.getLogger().addHandler(logging.StreamHandler(stream=sys.stdout))

query_engine = index.as_query_engine()
response = query_engine.query("저자는 어렸을때 어떤일을 했나요? 자세히 알려주세요.")
print(response)
```

> Batches: 100%|██████████| 1/1 [00:00<00:00,  1.82it/s]DEBUG:llama_index.core.indices.utils:> Top 2 nodes:
> 
> 
> > [Node a912fdd4-11ea-4c18-bb19-8f050568b93a] [Similarity score:             0.267923] What I Worked On
> > 
> 
> February 2021
> 
> Before college the two main things I worked on, outside of schoo...
> 
> > [Node b76f0983-0f46-4004-b2f1-27502286aa91] [Similarity score:             0.246462] One of the most conspicuous patterns I've noticed in my life is how well it has worked, for me at...
> Top 2 nodes:
> [Node a912fdd4-11ea-4c18-bb19-8f050568b93a] [Similarity score:             0.267923] What I Worked On
> > 
> 
> February 2021
> 
> Before college the two main things I worked on, outside of schoo...
> 
> > [Node b76f0983-0f46-4004-b2f1-27502286aa91] [Similarity score:             0.246462] One of the most conspicuous patterns I've noticed in my life is how well it has worked, for me at...
> > 
> 
> 저자는 어렸을 때 글쓰기와 프로그래밍을 했습니다. 특히 단편 소설을 썼지만, 그 당시에는 별로 좋지 않았다고 합니다. 또한 학교에서 사용하는 IBM 1401 컴퓨터를 이용하여 프로그래밍을 시작했습니다. 하지만 당시에는 컴퓨터 사용이 제한적이었고, 저자는 컴퓨터를 제대로 활용하지 못했습니다.
> 

### 인덱스 저장하기

현재 진행한 내용들은 로컬 매모리에 업로드 된 휘발성 데이터입니다. 이 임베딩을 디스크에 저장함으로써 시간과 비용을 아낄 수 있습니다.

```python
index.storage_context.persist()
```

저장된 인덱스는 다음과 같이 로드하여 사용할 수 있습니다.

```python
from llama_index.core import StorageContext, load_index_from_storage

PERSIST_DIR = "./storage"
storage_context = StorageContext.from_defaults(persist_dir=PERSIST_DIR)
index = load_index_from_storage(storage_context)

query_engine = index.as_query_engine()
response = query_engine.query("저자는 어렸을때 어떤일을 했나요? 자세히 알려주세요.")
print(response)
```