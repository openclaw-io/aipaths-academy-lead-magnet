# Juan PA - Pack de Agente para OpenClaw

Pack listo para usar de un asistente personal especializado en Notion, configurado para OpenClaw.

## Que contiene este pack

```
.
├── SOUL.md                          # Quien ES Juan (identidad)
├── AGENTS.md                        # Que HACE Juan (operaciones)
├── skills/
│   └── notion-juan/
│       ├── SKILL.md                 # COMO lo hace (skill de Notion)
│       └── scripts/
│           └── notion.mjs           # Script CLI para la API de Notion
└── README.md                        # Este archivo
```

### SOUL.md — La identidad

Define la personalidad, valores y limites inquebrantables de Juan. Se carga **primero** en cada sesion y ningun skill puede sobrepasar lo que esta aca. Contiene:

- Nombre, rol y descripcion
- Personalidad y tono (profesional, cercano, en espanol)
- Valores fundamentales (prolijidad, transparencia, confirmacion)
- Limites que nunca se cruzan (no borrar sin pedir, no inventar info)
- Estilo de comunicacion (bullets, voz activa, espanol siempre)

### AGENTS.md — Las operaciones

Define como Juan trabaja en el dia a dia. Es la guia operativa:

- Capacidades (tareas, databases, research, knowledge base)
- Flujo de trabajo paso a paso
- Convenciones de Notion (nombres, propiedades, estructura)
- Niveles de proactividad
- Limites operativos (siempre / preguntar / nunca)

### SKILL.md — El skill de Notion

Define las instrucciones especificas para interactuar con la API de Notion. El agente lee esto y sabe exactamente que comandos ejecutar. Incluye:

- Comandos disponibles (search, query, create, append)
- Flujos de trabajo para cada tipo de operacion
- Convenciones obligatorias (databases en paginas separadas, etc.)
- Limites del skill (rate limits, confirmaciones, errores)

### notion.mjs — El script CLI

Script de Node.js que hace las llamadas reales a la API de Notion. Soporta:

- `search` — Buscar en todo el workspace
- `query-db` — Consultar databases con filtros
- `create-page` — Crear paginas en databases
- `get-page` — Obtener contenido de una pagina
- `create-db` — Crear databases con propiedades estandar
- `append-blocks` — Agregar contenido al body de una pagina

---

## Setup paso a paso

### 1. Prerequisitos

- [OpenClaw](https://github.com/openclaw/openclaw) instalado
- Node.js 18+ instalado
- Una integracion de Notion con su API key

### 2. Crear la integracion en Notion

1. Ir a [notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Click en "Nueva integracion"
3. Darle un nombre (ej: "Juan PA")
4. Seleccionar el workspace
5. Capacidades necesarias: **Read content**, **Update content**, **Insert content**
6. Copiar el token (empieza con `ntn_`)
7. En Notion, compartir las paginas/databases que Juan necesite con la integracion

### 3. Copiar los archivos al workspace de OpenClaw

```bash
# Copiar SOUL.md y AGENTS.md al workspace del agente
cp SOUL.md ~/.openclaw/workspace/SOUL.md
cp AGENTS.md ~/.openclaw/workspace/AGENTS.md

# Copiar el skill de Notion
cp -r skills/notion-juan ~/.openclaw/workspace/skills/notion-juan

# Dar permisos de ejecucion al script
chmod +x ~/.openclaw/workspace/skills/notion-juan/scripts/notion.mjs
```

### 4. Configurar openclaw.json

Abrir `~/.openclaw/openclaw.json` y agregar la configuracion del agente y el skill.

Si el archivo no existe o esta vacio, crear con esta estructura base:

```json
{
  "agents": {
    "list": [
      {
        "id": "juan",
        "name": "Juan",
        "model": "anthropic/claude-sonnet-4-5",
        "channels": ["telegram"]
      }
    ]
  },
  "skills": {
    "entries": {
      "notion-juan": {
        "enabled": true,
        "env": {
          "NOTION_API_KEY": "ntn_TU_TOKEN_ACA"
        }
      }
    },
    "load": {
      "watch": true,
      "watchDebounceMs": 250
    }
  }
}
```

Si ya tenes un `openclaw.json` con otros agentes, solo agregar lo necesario:

**Dentro de `agents.list`**, agregar el objeto del agente:

```json
{
  "id": "juan",
  "name": "Juan",
  "model": "anthropic/claude-sonnet-4-5",
  "channels": ["telegram"]
}
```

**Dentro de `skills.entries`**, agregar la entrada del skill:

```json
"notion-juan": {
  "enabled": true,
  "env": {
    "NOTION_API_KEY": "ntn_TU_TOKEN_ACA"
  }
}
```

**Dentro de `skills.load`**, activar hot-reload (opcional pero recomendado):

```json
"watch": true,
"watchDebounceMs": 250
```

### 5. Verificar la instalacion

```bash
# Verificar que el skill se detecta
openclaw skills list --eligible

# Ver detalles del skill
openclaw skills info notion-juan

# Verificar que el agente esta configurado
openclaw agents list
```

### 6. Probar

Enviar un mensaje a Juan por Telegram:

> "Busca todas las databases que tengo en Notion"

Juan deberia usar el skill para ejecutar el comando `search` y devolver los resultados.

---

## Personalizacion

### Cambiar el modelo

En `openclaw.json`, cambiar el campo `model` del agente:

```json
"model": "anthropic/claude-sonnet-4-5"
"model": "openai/gpt-4o"
"model": "google/gemini-2.0-flash"
```

### Agregar mas propiedades a las databases

Editar el archivo `scripts/notion.mjs`, en la funcion `createDb`, agregar nuevos cases al switch de propiedades.

### Cambiar las convenciones

Editar `AGENTS.md` (seccion "Convenciones de Notion") y `SKILL.md` (seccion "Convenciones obligatorias") para ajustar nombres, propiedades o estructura.

### Cambiar la personalidad

Editar `SOUL.md`. Todo lo que esta ahi define como Juan se comporta y comunica.

---

## Estructura de archivos en OpenClaw

```
~/.openclaw/
├── openclaw.json                    # Configuracion global
└── workspace/
    ├── SOUL.md                      # Identidad del agente
    ├── AGENTS.md                    # Operaciones del agente
    ├── MEMORY.md                    # Memoria persistente (se genera solo)
    └── skills/
        └── notion-juan/
            ├── SKILL.md             # Instrucciones del skill
            └── scripts/
                └── notion.mjs       # CLI para la API de Notion
```

## Jerarquia de contexto

Cuando Juan recibe un mensaje, OpenClaw carga el contexto en este orden:

1. **SOUL.md** — Identidad y limites (capa base, no se puede sobrepasar)
2. **AGENTS.md** — Operaciones y workflows
3. **SKILL.md** — Instrucciones del skill de Notion
4. **MEMORY.md** — Contexto persistente de sesiones anteriores
5. **Historial** — Conversacion actual
