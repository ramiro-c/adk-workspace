# 🤖 AI Agents Hub

Monorepo de agentes de IA — experimentos con [Google ADK](https://google.github.io/adk-docs/) y [LangGraph](https://langchain-ai.github.io/langgraph/), y aplicaciones completas.

## 📁 Estructura

```
ai-agents-hub/
├── adk/                          # Agentes con Google ADK
│   ├── problem_solver/           #   PlanReActPlanner vía LiteLLM + OpenRouter
│   ├── my_first_agent/           #   Tutor de matemáticas (configuración Python)
│   ├── my_config_agent/          #   Tutor de matemáticas (configuración YAML)
│   ├── product_extractor/        #   Extractor de información de productos
│   └── programmatic_agent.py     #   Runner programático
│
├── langgraph/                    # Experimentos con LangGraph
│   └── lesson2.ipynb             #   Email Assistant (clasificación y draft)
│
├── customer-support-chat/        # App full-stack de soporte al cliente con ADK
│   ├── agent/                    #   Agente ADK (Alex Chen, soporte en español)
│   ├── backend/                  #   Proxy FastAPI entre el frontend y ADK
│   ├── frontend/                 #   UI de chat en React + TypeScript
│   └── scripts/dev.sh            #   Levanta los tres procesos en paralelo
│
├── .pre-commit-config.yaml       # Ruff lint + format en todos los .py
└── pyproject.toml                # Configuración de Ruff compartida
```

## 🚀 Cómo ejecutar

### ADK (Google Agent Development Kit)

Cada agente en `adk/` es un directorio independiente que se ejecuta con el CLI de ADK:

```bash
cd adk
source .venv/bin/activate

# Modo interactivo
adk run problem_solver

# Modo no interactivo (replay)
echo '{"state": {}, "queries": ["What is 2+2?"]}' | adk run --replay /dev/stdin problem_solver

# Web UI
adk web problem_solver
```

### LangGraph

```bash
cd langgraph
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Abrir `lesson2.ipynb` en Jupyter Notebook o VS Code.

### Customer Support Chat

App full-stack con tres procesos cooperando:

```
Browser (React :5173) → Vite proxy → FastAPI (:8080) → ADK api_server (:8000) → OpenRouter LLM
```

```bash
cd customer-support-chat
cp .env.example agent/.env   # agregar OPENROUTER_API_KEY
./scripts/dev.sh              # levanta los tres procesos
```

## 🔧 Pre-commit hooks

Los hooks de Ruff se ejecutan automáticamente en cada `git commit`:

- **lint**: `ruff check --fix` — corrige errores automáticos
- **format**: `ruff format` — formatea el código

Para correrlos manualmente:

```bash
adk/.venv/bin/pre-commit run --all-files
```

## 📦 Dependencias

Cada subdirectorio tiene su propio entorno:

| Directorio              | Runtime                                      |
| ----------------------- | -------------------------------------------- |
| `adk/`                  | Python 3.13, google-adk, LiteLLM             |
| `langgraph/`            | Python 3.13, langchain, langgraph            |
| `customer-support-chat` | Python 3 (agent + backend) + Node (frontend) |
