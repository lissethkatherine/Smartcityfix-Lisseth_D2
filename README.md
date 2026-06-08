# Smartcityfix-Lisseth_D2

# 🏙️ SmartCity-Fix

> Sistema automatizado de recepción, diagnóstico y enrutamiento de reportes de daños en infraestructura urbana.

---

## ¿Qué hace?

Un ciudadano reporta un daño (bache, luminaria, semáforo, etc.) desde un formulario web. El sistema lo analiza con IA, determina su prioridad y lo enruta automáticamente al equipo correcto, todo en tiempo real.

---

## Stack tecnológico

| Capa | Tecnología |
|------|-----------|
| Frontend | HTML · CSS · JavaScript (vanilla) |
| Automatización | n8n (webhook + flujo completo) |
| IA / NLP | Groq API — `llama-3.3-70b-versatile` |
| Geocodificación | OpenStreetMap Nominatim (inversa) |
| Base de datos | Google Sheets (OAuth2) |
| Notificaciones | Telegram Bot API |

---

## Flujo principal

```
Formulario web
    ↓
Webhook n8n → Sanitización de datos
    ↓
Geocodificación inversa (lat/lng → dirección legible)
    ↓
Anti-duplicados geográficos (Haversine, radio 20 m)
    ↓
IA: análisis NLP + filtro anti-spam visual
    ↓
Lógica de enrutamiento
    ├── 🔴 CRÍTICO        → Telegram emergencia inmediata
    ├── 🟡 MODERADO       → Telegram supervisores
    ├── 🟢 NORMAL         → Cola estándar
    └── ⚠️ SPAM/ARCHIVO   → Bloqueo + solicitud de evidencia
    ↓
Google Sheets (registro con 20 campos)
    ↓
Telegram → confirmación al ciudadano con Ticket ID
    ↓
HTTP 200 → formulario web muestra panel de éxito
```

---

## Funcionalidades implementadas

- ✅ Formulario web geolocalizado con validación completa
- ✅ Sanitización de inputs y archivos adjuntos (Sección 7)
- ✅ Geocodificación inversa automática vía OpenStreetMap
- ✅ Detección de duplicados geográficos (< 20 metros, fórmula Haversine)
- ✅ Análisis NLP con score de severidad 0–100 y nivel de confianza
- ✅ Filtro anti-spam visual (la IA detecta si la foto es de vía pública)
- ✅ Enrutamiento híbrido en 4 rutas (crítica / moderada / normal / spam)
- ✅ Bot de Telegram con `/consultar`, `/metricas` y menú de ayuda
- ✅ Panel de métricas en tiempo real: tasa de resolución, tiempo promedio de cierre, top categorías

---

## Bot de Telegram — Comandos

| Comando | Función |
|---------|---------|
| `/start` | Menú principal |
| `/consultar SCF-XXXXXX` | Estado de un ticket |
| `/metricas` | Panel de control en tiempo real |
| `/ayuda` | Lista de comandos |

---

## Estructura del repositorio

```
smartcityfix/
├── index.html                        # Formulario web (SPA)
├── workflow_smartcityfix_v2.json     # Workflow n8n (importar directo)
└── README.md                         # Este archivo
```

---

## Configuración rápida

1. **Importar el workflow** en tu instancia de n8n (`Settings → Import`)
2. **Configurar credenciales** en n8n:
   - `Groq API` — obtener key en [console.groq.com](https://console.groq.com)
   - `Google Sheets OAuth2` — autorizar con tu cuenta de Google
   - `Telegram Bot` — crear bot con [@BotFather](https://t.me/BotFather) y pegar el token
3. **Actualizar la URL del webhook** en `index.html` (variable `urlWebhookN8N`)
4. **Activar el workflow** y publicar `index.html` en GitHub Pages

---

## Google Sheets — Columnas requeridas

`TicketID · Fecha · Nombre · ID · Telefono · Correo · Categoría · Descripción · Dirección · Latitud · Longitud · Prioridad · Score · Confianza · Estado · Rutamiento · EntidadDestino · DiagnosticoIA · AccionRecomendada · FechaCierre · FotoURL`

---

## Autora

**Lisseth Katherine Zapata Cano**  
Técnica en Desarrollo de Software · CampusLands, Floridablanca — Santander  
[github.com/lissethkatherine](https://github.com/lissethkatherine) · [linkedin.com/in/lissethkatherine](https://linkedin.com/in/lissethkatherine)
