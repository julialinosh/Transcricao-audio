# Transcricao-audio
Conversando por Voz com ChatGPT utilizando Whisper e Python

Este projeto demonstra a integração de tecnologias de Inteligência Artificial para criar um assistente virtual capaz de interagir por voz. A solução utiliza Speech-to-Text, Processamento de Linguagem Natural e Text-to-Speech para proporcionar uma experiência de conversação natural e multilíngue.

# Descrição

O sistema permite que o usuário envie um áudio, que é transcrito em texto, processado por um modelo da OpenAI e convertido novamente em áudio. O projeto foi desenvolvido em Python e executado no Google Colab.
# ==============================================
# Conversação por Voz com OpenAI no Google Colab
# Speech-to-Text + ChatGPT + Text-to-Speech
# ==============================================

!pip install -q openai gtts

import os
from openai import OpenAI
from google.colab import files
from gtts import gTTS
from IPython.display import Audio, display

os.environ["OPENAI_API_KEY"] = input("Digite sua chave da OpenAI: ")

client = OpenAI()

print("\nFaça o upload de um arquivo de áudio (.wav ou .mp3):")
uploaded = files.upload()
nome_arquivo = list(uploaded.keys())[0]

with open(nome_arquivo, "rb") as audio:
    transcricao = client.audio.transcriptions.create(
        model="gpt-4o-transcribe",
        file=audio
    )

texto = transcricao.text
print("\nTranscrição:")
print(texto)

resposta = client.chat.completions.create(
    model="gpt-4.1-mini",
    messages=[
        {"role": "system", "content": "Você é um assistente útil e educado."},
        {"role": "user", "content": texto}
    ]
)

resposta_texto = resposta.choices[0].message.content
print("\nResposta do ChatGPT:")
print(resposta_texto)

tts = gTTS(text=resposta_texto, lang="pt")
tts.save("resposta.mp3")

print("\nÁudio gerado:")
display(Audio("resposta.mp3"))

files.download("resposta.mp3")
