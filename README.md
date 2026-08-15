# 🤖 Assistente Virtual Corporativo

Chatbot construído com **Streamlit** + **Groq (LLM)**, seguindo boas práticas de arquitetura de software e segurança em aplicações de IA.

🔗 **Demo:** https://agente-blueprints-securite-prompt-3.onrender.com

---

## ✨ Funcionalidades

- Chat em tempo real com streaming de respostas
- Histórico de conversa persistente durante a sessão
- Proteção contra Prompt Injection (Prompt Sandboxing com delimitadores XML)
- Sanitização de input do usuário
- Tratamento gracioso de erros e credenciais ausentes

## 🏗️ Arquitetura

O projeto segue uma separação clara de responsabilidades:

| Camada | Responsabilidade |
|---|---|
| `AppConfig` | Configuração e leitura segura de variáveis de ambiente |
| `InputSanitizer` | Limpeza e sanitização do texto digitado pelo usuário |
| `PromptSandbox` | Isolamento das instruções de sistema vs. input do usuário (anti-jailbreak) |
| `GroqService` | Toda a comunicação com a API da Groq, isolada da interface |
| `main()` / Streamlit | Camada de interface — não conhece detalhes da LLM |

## 🚀 Como rodar localmente

### Pré-requisitos
- Python 3.9+
- Uma chave de API da [Groq](https://console.groq.com) (gratuita)

### Passo a passo

```bash
# 1. Clone o repositório
git clone https://github.com/seu-usuario/seu-repositorio.git
cd seu-repositorio

# 2. Instale as dependências
pip install -r requirements.txt

# 3. Configure a variável de ambiente (Windows PowerShell)
$env:GROQ_API_KEY="sua_chave_aqui"

# Linux/Mac
export GROQ_API_KEY="sua_chave_aqui"

# 4. Rode a aplicação
streamlit run app.py
```

O app abre automaticamente em `http://localhost:8501`.

## ⚙️ Variáveis de ambiente

| Variável | Obrigatória | Descrição | Padrão |
|---|---|---|---|
| `GROQ_API_KEY` | ✅ Sim | Chave de API da Groq | — |
| `GROQ_MODEL` | ❌ Não | Modelo utilizado | `llama-3.3-70b-versatile` |
| `MAX_INPUT_CHARS` | ❌ Não | Limite de caracteres por mensagem | `4000` |
| `LLM_TEMPERATURE` | ❌ Não | Criatividade das respostas (0.0–1.0) | `0.4` |

> ⚠️ Nunca coloque a chave de API diretamente no código. Ela deve vir **sempre** de variáveis de ambiente.

## ☁️ Deploy no Render

1. Suba este repositório no GitHub
2. No [Render](https://dashboard.render.com), crie um novo **Web Service** apontando para o repositório
3. Configure:
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `streamlit run app.py --server.port $PORT --server.address 0.0.0.0`
4. Em **Environment**, adicione a variável `GROQ_API_KEY` com sua chave
5. Clique em **Deploy**

## 📦 Dependências

```
streamlit
groq
```

## 🔒 Segurança

Este projeto implementa:
- **Sanitização de input**: remoção de caracteres de controle, escape de HTML e limite de tamanho
- **Prompt Sandboxing**: delimitadores XML (`<SYSTEM_INSTRUCTIONS>` / `<USER_INPUT>`) que impedem o conteúdo do usuário de ser interpretado como instrução de sistema
- **Credenciais isoladas**: chaves de API lidas exclusivamente do ambiente, nunca hardcoded

## 📄 Licença

Este projeto é de uso livre para fins de estudo e aprendizado.

---

Desenvolvido como projeto de estudo em Engenharia de IA Aplicada.
