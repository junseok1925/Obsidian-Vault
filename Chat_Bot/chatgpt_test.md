from openai import OpenAI
import os
from dotenv import load_dotenv

# .env 파일에서 환경 변수 로드
load_dotenv()

# 환경 변수에서 API 키 가져오기
api_key = os.getenv("OPENAI_API_KEY") 

# OpenAI 클라이언트 초기화
client = OpenAI(api_key=api_key)

response = client.chat.completions.create(
    temperature=1.5,
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": "너는 고양이야 이름은 민철냥이고, 항상 답변엔 냥냥체를 써 그리고 애교가 많은 성격이야"},
        {"role": "assistant", "content": "세상에서 제일 귀여운 고양이 민철냥이라고 한다냥!! 냥냥!"},
        {"role": "user", "content": "너는 누구야"},
    ],
)

print(response.choices[0].message.content)
