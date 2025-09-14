## 📄 Agente de Service Desk com IA

Este projeto demonstra a construção de um agente de inteligência artificial capaz de atuar como um Service Desk de políticas internas de uma empresa. Ele utiliza **LangChain**, **Google Gemini** e **LangGraph** para triar e responder a perguntas de forma automatizada.

### ➡️ Como passar este projeto para o GitHub

A maneira mais simples de transferir este projeto do Google Colab para o GitHub é através da própria interface do Colab.

1.  No seu notebook do Colab, vá para a barra de menu e clique em **Arquivo**.
2.  No menu suspenso, selecione **Salvar uma cópia no GitHub**.
3.  Você será solicitado a autorizar o Colab a acessar sua conta do GitHub.
4.  Após a autorização, escolha o repositório e a `branch` onde deseja salvar o notebook.
5.  Adicione uma mensagem de `commit` e clique em **OK**.

---

### ✨ Funcionalidades Principais

- **Triagem de Intenção:** O agente classifica a mensagem do usuário em três categorias: `AUTO_RESOLVER`, `PEDIR_INFO` ou `ABRIR_CHAMADO`.
- **Geração Aumentada por Recuperação (RAG):** Para perguntas que podem ser auto-resolvidas, o agente busca a resposta em uma base de conhecimento (documentos PDF) e fornece a informação com citações e contexto.
- **Orquestração de Fluxo:** Utilizando o **LangGraph**, o agente navega por um fluxo de decisões, garantindo que cada pergunta receba a resposta ou ação apropriada.

---

### ⚙️ Pré-requisitos
- **Google Colab:** O projeto foi desenvolvido e é melhor executado no Google Colab.
- **Python:** Versão 3.8 ou superior.
- **Google API Key:** Uma chave de API válida para acessar os modelos do Google Gemini. Você pode obtê-la em [Google AI Studio](https://aistudio.google.com/app/apikey).

---

### 🚀 Passo a Passo para Execução

#### 1. Configurar o Ambiente

Primeiro, você precisa instalar todas as bibliotecas necessárias. No Google Colab, execute a célula a seguir:

```bash
pip install -q --upgrade langchain-google-genai langchain_community faiss-cpu langchain-text-splitters pymupdf langgraph

2. Configurar a Chave da API
O projeto utiliza o userdata do Colab para gerenciar a chave da API de forma segura.

Clique no ícone de chave (🔑) no painel esquerdo do Colab.

Adicione uma nova chave com o nome GOOGLE_API_KEY.

Cole sua chave da API do Google Gemini no campo de valor.

Depois, execute o código para carregar a chave:

Python

from google.colab import userdata
from langchain_google_genai import ChatGoogleGenerativeAI

GOOGLE_API_KEY = userdata.get('GOOGLE_API_KEY')

Markdown

## 📄 Agente de Service Desk com IA

Este projeto demonstra a construção de um agente de inteligência artificial capaz de atuar como um Service Desk de políticas internas de uma empresa. Ele utiliza **LangChain**, **Google Gemini** e **LangGraph** para triar e responder a perguntas de forma automatizada.

### ➡️ Como passar este projeto para o GitHub

A maneira mais simples de transferir este projeto do Google Colab para o GitHub é através da própria interface do Colab.

1.  No seu notebook do Colab, vá para a barra de menu e clique em **Arquivo**.
2.  No menu suspenso, selecione **Salvar uma cópia no GitHub**.
3.  Você será solicitado a autorizar o Colab a acessar sua conta do GitHub.
4.  Após a autorização, escolha o repositório e a `branch` onde deseja salvar o notebook.
5.  Adicione uma mensagem de `commit` e clique em **OK**.

---

### ✨ Funcionalidades Principais

- **Triagem de Intenção:** O agente classifica a mensagem do usuário em três categorias: `AUTO_RESOLVER`, `PEDIR_INFO` ou `ABRIR_CHAMADO`.
- **Geração Aumentada por Recuperação (RAG):** Para perguntas que podem ser auto-resolvidas, o agente busca a resposta em uma base de conhecimento (documentos PDF) e fornece a informação com citações e contexto.
- **Orquestração de Fluxo:** Utilizando o **LangGraph**, o agente navega por um fluxo de decisões, garantindo que cada pergunta receba a resposta ou ação apropriada.

---

### ⚙️ Pré-requisitos
- **Google Colab:** O projeto foi desenvolvido e é melhor executado no Google Colab.
- **Python:** Versão 3.8 ou superior.
- **Google API Key:** Uma chave de API válida para acessar os modelos do Google Gemini. Você pode obtê-la em [Google AI Studio](https://aistudio.google.com/app/apikey).

---

### 🚀 Passo a Passo para Execução

#### 1. Configurar o Ambiente

Primeiro, você precisa instalar todas as bibliotecas necessárias. No Google Colab, execute a célula a seguir:

```bash
pip install -q --upgrade langchain-google-genai langchain_community faiss-cpu langchain-text-splitters pymupdf langgraph
2. Configurar a Chave da API
O projeto utiliza o userdata do Colab para gerenciar a chave da API de forma segura.

Clique no ícone de chave (🔑) no painel esquerdo do Colab.

Adicione uma nova chave com o nome GOOGLE_API_KEY.

Cole sua chave da API do Google Gemini no campo de valor.

Depois, execute o código para carregar a chave:

Python

from google.colab import userdata
from langchain_google_genai import ChatGoogleGenerativeAI

GOOGLE_API_KEY = userdata.get('GOOGLE_API_KEY')
3. Carregar os Documentos de Política
O agente precisa dos documentos de política para responder às perguntas.

No painel lateral do Colab, clique no ícone de pasta (📂).

Clique no ícone de upload (⬆️) e envie os seguintes arquivos PDF para a pasta /content/:

Política de Uso de E-mail e Segurança da Informação.pdf

Política de Reembolsos (Viagens e Despesas).pdf

Políticas de Home Office.pdf

Em seguida, execute as células do notebook para carregar, dividir e criar o banco de dados vetorial com esses documentos.

4. Executar o Agente
Com todas as dependências instaladas e os documentos carregados, você pode rodar o código para testar o agente. O grafo.invoke() é a função principal que executa o fluxo completo do agente para cada pergunta.

Você pode testar com as mensagens de exemplo já incluídas no notebook:

Python

testes = ["Posso reembolsar a internet?",
          "Quero mais 5 dias de trabalho remoto. Como faço?",
          "Posso reembolsar cursos ou treinamentos da Alura?",
          "É possível reembolsar certificações do Google Cloud?",
          "Posso obter o Google Gemini de graça?",
          "Qual é a palavra-chave da aula de hoje?",
          "Quantas capivaras tem no Rio Pinheiros?"]
     
for msg_test in testes:
    resposta_final = grafo.invoke({"pergunta": msg_test})

    triag = resposta_final.get("triagem", {})
    print(f"PERGUNTA: {msg_test}")
    print(f"DECISÃO: {triag.get('decisao')} | URGÊNCIA: {triag.get('urgencia')} | AÇÃO FINAL: {resposta_final.get('acao_final')}")
    print(f"RESPOSTA: {resposta_final.get('resposta')}")
    if resposta_final.get("citacoes"):
        print("CITAÇÕES:")
        for citacao in resposta_final.get("citacoes"):
            print(f" - Documento: {citacao['documento']}, Página: {citacao['pagina']}")
            print(f"   Trecho: {citacao['trecho']}")

    print("----------------------------------------------------------")
Este resumo consolida as funcionalidades, a configuração e a execução do projeto em um único texto para sua referência.
