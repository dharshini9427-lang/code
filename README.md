from fastapi import FastAPI
from pydantic import BaseModel
import openai

app = FastAPI()

class CodeRequest(BaseModel):
    code: str
    language: str

@app.post("/analyze")
async def analyze_code(request: CodeRequest):
    prompt = f"""
    Analyze this {request.language} code for:
    1. Security issues
    2. Performance issues
    3. Code quality

    Return JSON with:
    - type
    - severity
    - issue
    - fix

    Code:
    {request.code}
    """

    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )

    return {"result": response["choices"][0]["message"]["content"]}
    
