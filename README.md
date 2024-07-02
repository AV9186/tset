python -m venv venv
source venv/bin/activate  # 对于Windows使用 venv\Scripts\activate

import openai
from flask import Flask, request, jsonify
import os

app = Flask(__name__)

# 从环境变量中获取API密钥
openai.api_key = os.getenv('OPENAI_API_KEY')

@app.route('/chat', methods=['POST'])
def chat():
    user_input = request.json.get('input')
    
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "You are a helpful assistant."},
            {"role": "user", "content": user_input}
        ]
    )
    
    reply = response['choices'][0]['message']['content']
    return jsonify({'reply': reply})

if __name__ == '__main__':
    app.run(debug=True)
