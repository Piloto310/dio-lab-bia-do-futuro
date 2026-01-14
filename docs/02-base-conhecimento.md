# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `base_golpes_conhecidos.json` | JASON | Repositório com padrões de frases, erros comuns e táticas de engenharia social. |
| `canais_oficiais_bradesco.csv` | CSV | Lista de domínios (.bradesco, .com.br), telefones e SMS oficiais para validação. |
| `historico_suspeito.csv` | CSV | Exemplos de mensagens enviadas por usuários que foram confirmadas como fraude. |

---

## Adaptações nos Dados

> Os dados foram expandidos com uma lista de "Palavras de Alerta" (como "urgente", "bloqueio", "imediatamente") e uma base de Typosquatting (variações visuais de links falsos). Também foram incluídos exemplos de golpes de "Falso Motoboy" e "Central de Segurança Falsa" para que o agente reconheça o modus operandi além do texto.
---

## Estratégia de Integração

### Como os dados são carregados?
> Os dados são carregados localmente através do framework LangChain. Utilizamos um TextLoader para os arquivos CSV/JSON, que são convertidos em documentos. Esses documentos são transformados em Embeddings e armazenados em um banco vetorial local (FAISS ou ChromaDB).

> from langchain_community.document_loaders import CSVLoader, JSONLoader
from langchain_community.vectorstores import FAISS
from langchain_community.embeddings import OllamaEmbeddings

# 1. Importando os Canais Oficiais
loader_canais = CSVLoader(file_path="./data/canais_oficiais_bradesco.csv")
docs_canais = loader_canais.load()

# 2. Criando o Banco de Dados Vetorial Local
embeddings = OllamaEmbeddings(model="llama3")
vector_db = FAISS.from_documents(docs_canais, embeddings)

# 3. Preparando o Recuperador (Retriever)
retriever = vector_db.as_retriever()

### Como os dados são usados no prompt?
> A consulta é feita via RAG (Retrieval-Augmented Generation). Quando o usuário cola uma mensagem, o sistema faz uma busca semântica na base de conhecimento. As informações mais relevantes (ex: "Como o Bradesco envia SMS?") são injetadas dinamicamente no contexto da LLM (Ollama/Llama 3) para que ela responda com base em fatos, não em suposições.
---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
CONTEXTO DE SEGURANÇA (Base de Conhecimento):
- Domínio Oficial: bradesco.com.br
- SMS Oficial: Não envia links de cancelamento.
- Tática Comum: Senso de urgência e ameaça de multa.

ENTRADA DO USUÁRIO:
"BRADESCO: Sua conta sera bloqueada em 30min por falta de atualizacao. Acesse: https://atualiza-bradesco.net"

ANÁLISE DO STEVE.AI:
1. Comparação de Link: "atualiza-bradesco.net" NÃO consta na lista de domínios oficiais.
2. Análise de Gatilho: Termos "bloqueada" e "30min" identificados como gatilhos de urgência.
3. Veredito: Risco de Phishing de 98%.
...
```
