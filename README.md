# 🎙️ Multimodal AI Agent - Speech & Text Processing Workflow

[![n8n](https://img.shields.io/badge/n8n-workflow-FF6D5A?logo=n8n)](https://n8n.io)
[![OpenAI](https://img.shields.io/badge/OpenAI-API-412991?logo=openai)](https://openai.com)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

> Yapay zeka destekli çoklu ortam işleme workflow'u. Speech-to-text, text-to-speech, ses klonlama ve AI-powered sosyal medya içerik üretimi özellikleri sunar.

## 📋 İçindekiler

- [Özellikler](#-özellikler)
- [Gereksinimler](#-gereksinimler)
- [Kurulum](#-kurulum)
- [Kullanım](#-kullanım)
- [API Referansı](#-api-referansı)
- [Örnekler](#-örnekler)
- [Mimari](#-mimari)
- [Katkıda Bulunma](#-katkıda-bulunma)
- [Lisans](#-lisans)

## ✨ Özellikler

### 🎯 Temel Fonksiyonlar

- **🗣️ Speech to Text**: Ses dosyalarını metne dönüştürme (OpenAI Whisper)
- **🔊 Text to Speech**: Metinleri doğal sesli konuşmaya çevirme (OpenAI TTS)
- **🎭 Voice Cloning**: Farklı ses tonları ile içerik üretme
- **📱 AI Post Generator**: GPT-4 ile otomatik sosyal medya içeriği oluşturma
- **📹 Video Processing**: Video dosyalarından ses çıkarma ve işleme

### 🚀 Teknik Özellikler

- RESTful API desteği
- Real-time processing
- Çoklu dil desteği (Türkçe optimizasyonu)
- Hata yönetimi ve logging
- Modüler mimari

## 🔧 Gereksinimler

### Yazılım

- [n8n](https://n8n.io/) (v1.0+)
- Node.js (v18+)
- npm veya yarn

### API Keys

- [OpenAI API Key](https://platform.openai.com/api-keys) (GPT-4 ve Whisper erişimi)
- (Opsiyonel) ElevenLabs API Key (gelişmiş ses klonlama için)

### Sistem Gereksinimleri

- **RAM**: Minimum 4GB (8GB önerilir)
- **Storage**: 1GB boş alan
- **İnternet**: Stabil bağlantı (API çağrıları için)

## 📦 Kurulum

### 1. Repository'yi Klonlayın

```bash
git clone https://github.com/Srhot/text-to-speech-to_text.git
cd text-to-speech-to_text
```

### 2. n8n'i Başlatın

**Docker ile:**

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

**npm ile:**

```bash
npm install n8n -g
n8n start
```

### 3. Workflow'u İçe Aktarın

1. Tarayıcınızda `http://localhost:5678` adresini açın
2. Sol menüden **Workflows** → **Import** seçin
3. `workflow.json` dosyasını seçin
4. **Import** butonuna tıklayın

### 4. API Credential'larını Ekleyin

#### OpenAI API Ayarları

1. Workflow'da herhangi bir OpenAI node'una tıklayın
2. **Credential to connect with** dropdown'ı açın
3. **Create New** → **OpenAI API** seçin
4. API Key'inizi girin:
   ```
   API Key: sk-proj-XXXXXXXXXXXXXXXXX
   ```
5. **Save** butonuna tıklayın

### 5. Workflow'u Aktif Edin

- Sağ üst köşedeki **Inactive** toggle'ını **Active** yapın
- Webhook URL'i otomatik olarak oluşturulacaktır

## 💻 Kullanım

### Webhook URL

Workflow aktif edildikten sonra webhook URL'niz:

```
http://localhost:5678/webhook/multimedia-ai
```

### Temel Kullanım

Tüm istekler **POST** metodu ile JSON formatında gönderilmelidir:

```bash
curl -X POST http://localhost:5678/webhook/multimedia-ai \
  -H "Content-Type: application/json" \
  -d '{"action": "text_to_speech", "text": "Merhaba dünya"}'
```

## 📚 API Referansı

### 1. Text to Speech

**Metin → Ses dönüşümü**

**Request:**
```json
{
  "action": "text_to_speech",
  "text": "Dönüştürülecek metin",
  "voice": "nova",
  "speed": 1.0
}
```

**Parametreler:**
- `text` (string, zorunlu): Sese dönüştürülecek metin
- `voice` (string, opsiyonel): Ses tonu - `alloy`, `echo`, `fable`, `onyx`, `nova`, `shimmer`
- `speed` (float, opsiyonel): Konuşma hızı (0.25 - 4.0 arası)

**Response:**
```json
{
  "status": "success",
  "action": "text_to_speech",
  "result": {
    "audio_url": "base64_encoded_audio_data",
    "format": "mp3",
    "duration": "3.5s"
  },
  "timestamp": "2025-10-20T12:34:56.789Z"
}
```

**cURL Örneği:**
```bash
curl -X POST http://localhost:5678/webhook/multimedia-ai \
  -H "Content-Type: application/json" \
  -d '{
    "action": "text_to_speech",
    "text": "Bu bir test mesajıdır.",
    "voice": "nova",
    "speed": 1.2
  }'
```

---

### 2. Speech to Text

**Ses → Metin dönüşümü**

**Request:**
```json
{
  "action": "speech_to_text",
  "audio_url": "https://example.com/audio.mp3",
  "language": "tr"
}
```

**veya Base64 ile:**
```json
{
  "action": "speech_to_text",
  "audio_base64": "data:audio/mp3;base64,//uQx...",
  "language": "tr"
}
```

**Parametreler:**
- `audio_url` veya `audio_base64` (string, zorunlu): Ses dosyası
- `language` (string, opsiyonel): Dil kodu (ISO 639-1) - varsayılan: `tr`

**Desteklenen Formatlar:**
- MP3, MP4, MPEG, MPGA, M4A, WAV, WEBM

**Response:**
```json
{
  "status": "success",
  "action": "speech_to_text",
  "result": {
    "text": "Transkribe edilmiş metin içeriği",
    "language": "tr",
    "duration": "45.2s",
    "word_count": 125
  },
  "timestamp": "2025-10-20T12:34:56.789Z"
}
```

**cURL Örneği:**
```bash
curl -X POST http://localhost:5678/webhook/multimedia-ai \
  -H "Content-Type: application/json" \
  -d '{
    "action": "speech_to_text",
    "audio_url": "https://example.com/sample.mp3",
    "language": "tr"
  }'
```

---

### 3. Voice Clone

**Özel ses tonu ile içerik üretimi**

**Request:**
```json
{
  "action": "voice_clone",
  "text": "Klonlanacak ses ile söylenecek metin",
  "voice_profile": "professional",
  "emotion": "neutral"
}
```

**Parametreler:**
- `text` (string, zorunlu): Söylenecek metin
- `voice_profile` (string, opsiyonel): `professional`, `casual`, `energetic`, `calm`
- `emotion` (string, opsiyonel): `neutral`, `happy`, `sad`, `excited`

**Response:**
```json
{
  "status": "success",
  "action": "voice_clone",
  "result": {
    "audio_url": "base64_encoded_audio",
    "voice_characteristics": {
      "profile": "professional",
      "emotion": "neutral",
      "pitch": "medium"
    }
  },
  "timestamp": "2025-10-20T12:34:56.789Z"
}
```

---

### 4. AI Post Generator

**Yapay zeka ile sosyal medya içeriği üretimi**

**Request:**
```json
{
  "action": "create_post",
  "topic": "Yapay zeka ve eğitim",
  "platform": "twitter",
  "tone": "professional",
  "include_hashtags": true
}
```

**Parametreler:**
- `topic` (string, zorunlu): İçerik konusu
- `platform` (string, opsiyonel): `twitter`, `linkedin`, `instagram`, `facebook`
- `tone` (string, opsiyonel): `professional`, `casual`, `humorous`, `inspiring`
- `include_hashtags` (boolean, opsiyonel): Hashtag eklensin mi?
- `language` (string, opsiyonel): İçerik dili - varsayılan: `tr`

**Response:**
```json
{
  "status": "success",
  "action": "create_post",
  "result": {
    "content": "🚀 Yapay zeka eğitimde devrim yaratıyor...",
    "hashtags": ["#AI", "#Eğitim", "#Teknoloji"],
    "character_count": 245,
    "platform": "twitter",
    "suggested_images": ["ai-education-1.jpg"]
  },
  "timestamp": "2025-10-20T12:34:56.789Z"
}
```

**cURL Örneği:**
```bash
curl -X POST http://localhost:5678/webhook/multimedia-ai \
  -H "Content-Type: application/json" \
  -d '{
    "action": "create_post",
    "topic": "Yapay zeka ve gelecek",
    "platform": "linkedin",
    "tone": "professional",
    "include_hashtags": true
  }'
```

---

### 5. Video Processing

**Video'dan ses çıkarma ve işleme**

**Request:**
```json
{
  "action": "process_video",
  "video_url": "https://example.com/video.mp4",
  "extract_audio": true,
  "transcribe": true
}
```

**Parametreler:**
- `video_url` (string, zorunlu): Video dosyası URL'i
- `extract_audio` (boolean, opsiyonel): Ses çıkarılsın mı?
- `transcribe` (boolean, opsiyonel): Çıkarılan ses transkribe edilsin mi?

**Response:**
```json
{
  "status": "success",
  "action": "process_video",
  "result": {
    "audio_url": "extracted_audio.mp3",
    "transcript": "Video içeriğinin tam transkripti...",
    "duration": "2:34",
    "video_metadata": {
      "resolution": "1920x1080",
      "fps": 30,
      "codec": "h264"
    }
  },
  "timestamp": "2025-10-20T12:34:56.789Z"
}
```

---

## 🎯 Örnekler

### Python ile Kullanım

```python
import requests
import json

# Workflow endpoint
url = "http://localhost:5678/webhook/multimedia-ai"

# Text to Speech örneği
def text_to_speech(text, voice="nova"):
    payload = {
        "action": "text_to_speech",
        "text": text,
        "voice": voice,
        "speed": 1.0
    }
    
    response = requests.post(url, json=payload)
    return response.json()

# Speech to Text örneği
def speech_to_text(audio_url):
    payload = {
        "action": "speech_to_text",
        "audio_url": audio_url,
        "language": "tr"
    }
    
    response = requests.post(url, json=payload)
    return response.json()

# AI Post Generator örneği
def create_social_post(topic, platform="twitter"):
    payload = {
        "action": "create_post",
        "topic": topic,
        "platform": platform,
        "tone": "professional",
        "include_hashtags": True
    }
    
    response = requests.post(url, json=payload)
    return response.json()

# Kullanım
if __name__ == "__main__":
    # TTS örneği
    result = text_to_speech("Merhaba, bu bir test mesajıdır")
    print("TTS Result:", result)
    
    # Post oluşturma
    post = create_social_post("Yapay zeka ve eğitim", "linkedin")
    print("Generated Post:", post['result']['content'])
```

### JavaScript/Node.js ile Kullanım

```javascript
const axios = require('axios');

const API_URL = 'http://localhost:5678/webhook/multimedia-ai';

// Text to Speech
async function textToSpeech(text, voice = 'nova') {
  try {
    const response = await axios.post(API_URL, {
      action: 'text_to_speech',
      text: text,
      voice: voice,
      speed: 1.0
    });
    return response.data;
  } catch (error) {
    console.error('Error:', error.message);
  }
}

// Speech to Text
async function speechToText(audioUrl, language = 'tr') {
  try {
    const response = await axios.post(API_URL, {
      action: 'speech_to_text',
      audio_url: audioUrl,
      language: language
    });
    return response.data;
  } catch (error) {
    console.error('Error:', error.message);
  }
}

// AI Post Generator
async function createPost(topic, platform = 'twitter') {
  try {
    const response = await axios.post(API_URL, {
      action: 'create_post',
      topic: topic,
      platform: platform,
      tone: 'professional',
      include_hashtags: true
    });
    return response.data;
  } catch (error) {
    console.error('Error:', error.message);
  }
}

// Kullanım örnekleri
(async () => {
  // TTS
  const ttsResult = await textToSpeech('Merhaba dünya!');
  console.log('TTS Result:', ttsResult);
  
  // AI Post
  const post = await createPost('Yapay zeka geleceği', 'linkedin');
  console.log('Generated Post:', post.result.content);
})();
```

### Postman Collection

Postman için hazır collection dosyası: [`postman_collection.json`](./postman_collection.json)

**Import Adımları:**
1. Postman'i açın
2. **Import** → **Upload Files**
3. `postman_collection.json` dosyasını seçin
4. Hazır 5 örnek istek ile test edebilirsiniz

## 🏗️ Mimari

### Workflow Yapısı

```
┌─────────────┐
│   Webhook   │  ← HTTP POST istekleri
│   Trigger   │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Router    │  ← Action type kontrolü
│   (Switch)  │
└──────┬──────┘
       │
       ├──────────────┬──────────────┬──────────────┬──────────────┐
       ▼              ▼              ▼              ▼              ▼
 ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
 │   TTS    │  │   STT    │  │  Voice   │  │   AI     │  │  Video   │
 │          │  │          │  │  Clone   │  │   Post   │  │ Process  │
 └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘
      │             │             │             │             │
      └─────────────┴─────────────┴─────────────┴─────────────┘
                              ▼
                      ┌──────────────┐
                      │   Response   │
                      │  Formatter   │
                      └──────────────┘
```

### Node Detayları

| Node | Tip | Fonksiyon |
|------|-----|-----------|
| Webhook Trigger | n8n-nodes-base.webhook | HTTP isteklerini alır |
| Router | n8n-nodes-base.switch | Action type'a göre yönlendirir |
| Text to Speech | n8n-nodes-base.httpRequest | OpenAI TTS API |
| Speech to Text | n8n-nodes-base.httpRequest | OpenAI Whisper API |
| Voice Clone | n8n-nodes-base.httpRequest | OpenAI TTS (farklı voices) |
| AI Post Generator | n8n-nodes-base.httpRequest | OpenAI GPT-4 API |
| Video Processor | n8n-nodes-base.httpRequest | FFmpeg + Whisper |
| Response Formatter | n8n-nodes-base.set | Yanıtı formatlar |

### Veri Akışı

```javascript
Request → Validation → Processing → AI API → Response
   ↓          ↓           ↓          ↓         ↓
 JSON      Action      OpenAI    Result    Formatted
 Body      Check       Call      Parse      JSON
```

## 🐛 Hata Ayıklama

### Yaygın Hatalar ve Çözümleri

#### 1. "Invalid API Key" Hatası

**Sorun:** OpenAI API key geçersiz veya eksik

**Çözüm:**
```bash
# API key'inizi kontrol edin
# https://platform.openai.com/api-keys
# Node credential'larını yeniden yapılandırın
```

#### 2. "Webhook not responding"

**Sorun:** Workflow aktif değil veya n8n çalışmıyor

**Çözüm:**
```bash
# n8n'in çalıştığını kontrol edin
curl http://localhost:5678

# Workflow'un aktif olduğunu kontrol edin
# n8n UI → Workflow → Toggle: Active
```

#### 3. "Audio file format not supported"

**Sorun:** Desteklenmeyen ses formatı

**Çözüm:**
- Desteklenen formatlar: MP3, MP4, WAV, WEBM
- FFmpeg ile dönüştürme:
```bash
ffmpeg -i input.ogg -acodec mp3 output.mp3
```

### Debug Mode

n8n'i debug mode'da çalıştırma:

```bash
export N8N_LOG_LEVEL=debug
n8n start
```

### Log Kontrol

```bash
# n8n logları
tail -f ~/.n8n/n8n.log

# Workflow execution logs
# n8n UI → Executions → İlgili execution → View
```

## 🤝 Katkıda Bulunma

Katkılarınızı bekliyoruz! Lütfen aşağıdaki adımları izleyin:

### 1. Fork ve Clone

```bash
# Repository'yi fork edin (GitHub UI)
git clone https://github.com/YOUR_USERNAME/multimodal-ai-workflow.git
cd multimodal-ai-workflow
```

### 2. Branch Oluşturun

```bash
git checkout -b feature/amazing-feature
```

### 3. Değişikliklerinizi Yapın

```bash
# Kodunuzu yazın
# Test edin
git add .
git commit -m "feat: Add amazing feature"
```

### 4. Push ve Pull Request

```bash
git push origin feature/amazing-feature
# GitHub'da Pull Request oluşturun
```

### Commit Message Kuralları

```
feat: Yeni özellik
fix: Bug düzeltme
docs: Dokümantasyon
style: Formatlama
refactor: Kod iyileştirme
test: Test ekleme
chore: Bakım işleri
```

## 📖 Ek Kaynaklar

### Dokümantasyon

- [n8n Documentation](https://docs.n8n.io/)
- [OpenAI API Reference](https://platform.openai.com/docs/api-reference)
- [Workflow Best Practices](https://docs.n8n.io/workflows/workflows/)

### Video Tutoriallar

- [n8n Başlangıç Rehberi](https://www.youtube.com/watch?v=1MwSoB0gnM4)
- [OpenAI API Kullanımı](https://www.youtube.com/watch?v=example)

### Community

- [n8n Community Forum](https://community.n8n.io/)
- [Discord Server](https://discord.gg/n8n)

## 📄 Lisans

Bu proje MIT lisansı altında lisanslanmıştır. Detaylar için [LICENSE](LICENSE) dosyasına bakın.

```
MIT License

Copyright (c) 2025 [Your Name]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction...
```

## 👤 İletişim

**Proje Sorumlusu:** [Serhat SEZGÜL]

- GitHub: srhot(https://github.com/srhot)


## 🙏 Teşekkürler

- [n8n](https://n8n.io/) - Workflow automation platformu
- [OpenAI](https://openai.com/) - AI API'leri
- Tüm katkıda bulunanlara

---

<div align="center">

**⭐ Projeyi beğendiyseniz yıldız vermeyi unutmayın!**

Made with ❤️ by [Your Name]

</div>
