# Sabores & Eventos · Asistente IA en n8n

Solución de automatización con IA para **Sabores & Eventos**, un restaurante y empresa de
catering de fusión venezolana-chilena en Santiago. El proyecto resuelve la atención de
consultas por Telegram (carta, paquetes, precios, disponibilidad, referencias visuales y
notas de voz) mediante un agente de IA con memoria, herramientas y guardrails, construido
en **n8n**.

> Evaluación práctica — Clase 22, Desafío Latam. Autora: **Ruth Velásquez**.

## 🌐 Sitio web

- `index.html` — Landing pública del negocio (hero, propuesta, paquetes, menú, servicios,
  FAQ, políticas, horarios y contacto). Diseño responsivo y autocontenido.
- `faq.html` — Versión ligera de solo texto (FAQ + políticas + horarios + servicios) que
  consume el bot mediante el nodo HTTP Request (`consultar_web_faq`).

## 🤖 El asistente (n8n)

Chatbot de Telegram multimodal con un agente de IA que:

- Responde preguntas usando una **base de conocimiento** (Google Sheets + página web).
- Mantiene **memoria por usuario** (PostgreSQL en Supabase, sessionKey = chat_id).
- **Agenda reuniones** de planificación/degustación en Google Calendar.
- Envía **confirmaciones por email** (Gmail, plantilla HTML de marca).
- Usa la herramienta **Think** para razonar antes de actuar.
- Procesa **imágenes** (visión + Structured Output Parser → registro en Sheets).
- Procesa **notas de voz** (transcripción con Groq Whisper).
- Registra clientes/leads en una tabla de **Supabase** (`clientes_sabores`).
- Incluye **guardrails** anti prompt-injection, off-topic y datos confidenciales.

### Modelos usados

| Función | Modelo |
|---|---|
| Agente conversacional | Claude Sonnet 4.6 (Anthropic) |
| Visión de imágenes | Claude Haiku 4.5 (Anthropic) |
| Transcripción de voz | Whisper-large-v3 (Groq) |

## 📁 Estructura

```
.
├── index.html                       # Landing pública
├── faq.html                         # Página de FAQ/políticas (la lee el bot)
├── README.md
├── docs/
│   └── Diseno_Prompts_y_Guardrails.pdf   # Documento de diseño (entregable)
├── n8n/
│   └── workflow_sabores_eventos.json     # Workflow exportado (sin API keys)
└── data/
    └── Sabores_Eventos_Base_Conocimiento.xlsx
```

## 🚀 Despliegue del sitio

Sitio estático, sin build. Sirve `index.html` directamente.

- **Vercel:** importar el repo → Deploy (sin framework).
- **GitHub Pages:** Settings → Pages → rama `main`.

## ⚠️ Notas de seguridad

- El workflow exportado **no incluye credenciales** (Telegram, Supabase, Google, Anthropic);
  n8n exporta solo referencias a credenciales, no los secretos.
- La API key de Groq fue **reemplazada por un placeholder** (`TU_API_KEY_GROQ`) en el JSON
  exportado. Reemplázala por la tuya al importar.

## 🛠️ Cómo importar el workflow en n8n

1. n8n → *Workflows* → *Import from File* → `n8n/workflow_sabores_eventos.json`.
2. Reconectar las credenciales (Telegram, Supabase/Postgres, Google, Anthropic, Groq).
3. Publicar (Publish) para activar el webhook de Telegram.
