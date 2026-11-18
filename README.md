<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:4A0D2E,50:C2185B,100:FF8FC7&height=190&section=header&text=STS%20Module&fontSize=52&fontColor=FFFFFF&fontAlignY=36&desc=Talk%20to%20an%20AI%20and%20hear%20it%20answer%2C%20no%20typing&descSize=16&descAlignY=58" width="100%" alt="STS Module"/>

<p>
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python"/>
  <img src="https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white" alt="OpenAI"/>
  <img src="https://img.shields.io/badge/Backend%20only-FF4FA3?style=for-the-badge" alt="Backend only"/>
</p>

</div>

---

## Why it exists

Some users cannot or do not want to type: someone on the move, someone with low literacy, or anyone who simply finds speaking faster. **STS (speech to speech)** closes the loop: the user speaks, the AI understands, thinks and answers out loud.

It is one of three reusable voice modules I packaged during my Conversational AI internship at Prudential Solutions, where I evaluated and benchmarked GPT-based speech components for an enterprise HR chatbot. It combines the ideas of its two siblings, [STT_module](https://github.com/21Afnan/STT_module) (speech to text) and [TTS_module](https://github.com/21Afnan/TTS_module) (text to speech), into one conversation pipeline.

## How a turn works

```
User's voice (audio file)
      │  speech_to_text()      OpenAI transcription
      ▼
Text question  ──>  get_answer(messages)      gpt-4o-mini, with the conversation so far
                         │
                         ▼
               Text answer  ──>  text_to_speech()      OpenAI TTS (tts-1, voice "nova")
                                       │
                                       ▼
                         response.mp3  ──>  audio_to_base64()  ──>  any frontend can play it
```

## Functions

All logic is in `speech_chatbot/chatbot.py`, as four small functions you can call from any app:

| Function | Input | Output |
|---|---|---|
| `speech_to_text(audio_path)` | path to an audio file | transcribed text |
| `get_answer(messages)` | list of chat messages | the assistant's reply (a system prompt is added for you) |
| `text_to_speech(text, output_path)` | reply text | path of the saved MP3 (default `temp_audio/response.mp3`) |
| `audio_to_base64(file_path)` | MP3 path | base64 string, ready to embed in a web page or JSON |

Because it is **backend only**, it can be wired into a web app, a desktop app, a mobile backend or an automation script.

## Quick start

```bash
git clone https://github.com/21Afnan/STS_module.git
cd STS_module
pip install -r requirements.txt
```

Create a `.env` file with your key:

```
OPENAI_API_KEY=your_key_here
```

Then, in Python:

```python
from speech_chatbot.chatbot import speech_to_text, get_answer, text_to_speech, audio_to_base64

question = speech_to_text("question.wav")
history = [{"role": "user", "content": question}]
answer = get_answer(history)
mp3_path = text_to_speech(answer)
audio_b64 = audio_to_base64(mp3_path)   # send this to the browser
```

Keep appending user and assistant messages to `history` to hold a multi-turn conversation.

## Project structure

```
STS_module/
├── speech_chatbot/
│   ├── __init__.py
│   └── chatbot.py      # speech to text, answer, text to speech, base64
└── requirements.txt    # openai, python-dotenv
```

---

<div align="center">

**Afnan Shoukat**

<a href="mailto:afnanshoukat35@gmail.com"><img src="https://img.shields.io/badge/Email-afnanshoukat35%40gmail.com-FF4FA3?style=for-the-badge&logo=gmail&logoColor=white&labelColor=4A0D2E" alt="Email"/></a>
<a href="https://www.linkedin.com/in/afnanshoukat"><img src="https://img.shields.io/badge/LinkedIn-Afnan%20Shoukat-FF4FA3?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=4A0D2E" alt="LinkedIn"/></a>
<a href="https://github.com/21Afnan"><img src="https://img.shields.io/badge/GitHub-21Afnan-FF4FA3?style=for-the-badge&logo=github&logoColor=white&labelColor=4A0D2E" alt="GitHub"/></a>

</div>

