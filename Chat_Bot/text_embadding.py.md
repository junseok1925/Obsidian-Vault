```python

import pandas as pd # 데이터를 다루기 위한 라이브러리, DataFrame 형식으로 데이터를 쉽게 조작 할 수 있음. pd라는 별칭으로 사용

import tiktoken # OpenAI API에서 사용하는 토큰화(Tokenization) 라이브러리 문자열을 "토큰" 단위로 변환하는 역할 ex) "나는 강준석입니다" -> ["나는", "강준석", "입니다"]

from openai import OpenAI # OpenAI API를 사용하기 위해 OpenAI 클래스를 가져온다.

from typing import List # List 타입을 명확하게 지정하기 위한 모듈, 함수의 인자나 반환값이 리스트일 때, List[str], List[int]등의 타입 힌트를 사용할 수 있다.

  

client = OpenAI() # OpenAI를 사용하기 위한 클라이언트 객체를 생성

  

embedding_model = "text-embedding-3-small" # OpenAI에서 제공하는 텍스트 임베딩 모델 지정, 입력된 문장을 숫자 벡터(embedding)로 변환하는 역할, 예를 들어 "I love cats" → [0.234, -0.912, 1.233, ...] 같은 숫자 리스트로 변환.

embedding_encoding = "cl100k_base" # OpenAI의 최신 토큰 인코딩 방식

max_tokens = 1500 # 한 번의 임베딩 요청에 처리할 수 있는 최대 토큰 수를 제한 -> 너무 많은  토큰을 입력하면 비용이 증가하고 응답 속도가 느려질 수 있음

  

# 'scraped.csv' 파일을 불러와서 칼럼 이름을 'title'과 text'로 변경

df = pd.read_csv('scraped.csv') # 현재 디렉토리에 있는 scraped.csv 파일을 읽어 pandas 데이터프레임워크(df)로 저장

df.columns = ['title', 'text'] # scraped.csv 파일의 컬럼 이름을 'title', 'text'로 변경

  

tokenizer = tiktoken.get_encoding(embedding_encoding)

df['n_tokens'] = df.text.apply(lambda x: len(tokenizer.encode(x))) # df['text'] 열의 각 문장을 tokenizer.encode(x)를 사용해 토큰 리스트로 변환

                                                                      # 그리고 len() 함수를 사용해 토큰 리스트의 길이를 구하여 'n_tokens'이라는 새로운 열에 저장 즉, 각 텍스트가 몇 개의 토큰으로 구성되어 있는지 계산하는 과정

def split_into_many (text, max_tokens= 500): # 긴 문장을 500 토큰 이하의 여러 개의 작은 문장으로 나누는 역할을 함.

  

    # 텍스트를 문장별로 나누어 각 문장의 토큰 개수를 구함

    sentences = text.split('.') # 마침표(.)를 기준으로 문장을 나눔 "Hello world. How are you?" → ["Hello world", " How are you?"] 즉, 한 문장이 너무 길 경우, 문장 단위로 나누어 처리하려는 목적적

    n_tokens = [len(tokenizer.encode(" " + sentence)) for sentence in sentences] # split된 각 문장을 토큰화 하고, 해당 문장의 토큰 개수를 리스트로 저장,

  

      # sentences와 n_tokens를 print로 출력

    print("Sentences:", sentences)  # sentences 출력

    print("Token counts:", n_tokens)  # n_tokens 출력

  

    chunks = [] # 나중에 max_tokens 기준을 초과하지 않는 문자 묶음(청크)을 저장할 리스트 -> 최종적으로 chunks 리스트안에 여러 개의 작은 텍스트 조각이 들어감

    tokens_so_far = 0 # 현재까지 chunk 안에 들어간 문장들의 총 토큰 갯수

    chunk = [] # 현재 만들고 있는 작은 문장 그룹 (청크)

  

    # 각 문장과 토큰을 결합해 루프 처리

    for sentence, token in zip(sentences, n_tokens): # sentences 리스트와 n_tokens 리스트를 zip 함수를 사용해 묶어 루프를 돌려 각 문장과 해당 문장의 토큰 개수를 한 쌍식 가져옴

  

        # 지금까지의 토큰 수와 현재 문장의 토큰 수를 합한 값이

        # 최대 토큰 수를 초과하면 청크를 청크 목록에 추가하고

        # 청크 및 토큰 수를 재설정

        if tokens_so_far + token > max_tokens: # 현재까지의 토큰 수(tokens_so_far)에 현재 문장의 토큰 수(token)을 더한 값이 max_tokens(1500)을 초과하는 경우

            chunks.append(". ".join(chunk) + ".") # 지금까지 모은 문장들을 하나의 chunk로 묶어서 chunks 리스트에 추가

            chunk = []                            # chunk를 초기화

            tokens_so_far = 0                     # tokens_so_far를 초기화

  

            # 현재 문장의 토큰 수가 최대 토큰 수 보다 크면 다음 문장으로 넘어감

        if token > max_tokens:

            continue

  

            # 그렇지 않은 경우, 문장을 청크해 추가하고 토큰 수를 합계에 추가

        chunk.append(sentence) # 현재 문장(sentence)을 chunk 리스트트에 추가

        tokens_so_far += token + 1 # 현재 문장의 토큰 수를 tokens_so_far에 더함. +1은 토큰화 방법에서 문장의 끝에 공백 토큰이 추가되므로, 이 공백을 고려하여 +1을 해주는 것

  

        # 마지막 청크를 청크 목록에 추가

        # 루프가 끝난 후, chunk에 남아있는 문장들이 있으면 그것도 마지막 청크로 추가한다

    if chunk:

        chunks.append(". ".join(chunk) + ".")

    return chunks

    # 축약된 텍스트를 저장하기 위한 리스트``

shortened = []

  

    # DataFrmae의 각 행에 대한 루프 처리

for row in df.iterrows():

        # 텍스트가 None인 경우 다음 줄로 넘어감

    if row[1]['text'] is None:

            continue

        # 토큰 수가 최대 토큰 수보다 큰 경우, 텍스트를

        # shortened 리스트에 추가

    if row[1]['n_tokens'] > max_tokens:

        shortened += split_into_many(row[1]['text'])

  

        # 그 외의 경우 텍스트를 그대로 'shortened' 리스트에 추가

    else:

        shortened.append(row[1]['text'])

  

            #"shortened"를 기반으로 새로운 DataFrame을 생성하고, 열 이름을 "text"로 지정

df = pd.DataFrame(shortened, columns=['text'])

  

            # 각 'text'의 토큰 수를 계산하여 새로운 열 'n_token'에 저장

df['n_tokens'] = df.text.apply(lambda x: len(tokenizer.encode(x)))

  

            # 'text' 열의 텍스트에 대하 embedding을 수행하여 csv 파일로 저장

def get_embedding(text, model):

    text = text.replace("\n", " ")

    return client.embeddings.create(input = [text], model=model).data[0].embedding

            # 'text' 열의 텍스트에 대해 embedding을 수행하여 csv 파일로 저장장

df["embeddings"] = df.text.apply(lambda x : get_embedding(x, model=embedding_model))

df.to_csv('embeddings.csv')
```