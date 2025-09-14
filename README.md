# 🤖 Agente de Service Desk Inteligente com IA

Bem-vindo ao repositório do Agente de Service Desk Inteligente! Este projeto inovador demonstra a aplicação de inteligência artificial para automatizar e otimizar o atendimento de Service Desk, focando especificamente na gestão de políticas internas de uma empresa. Utilizando as poderosas ferramentas **LangChain**, **Google Gemini** e **LangGraph**, este agente é capaz de triar, compreender e responder a uma vasta gama de perguntas de forma autônoma e eficiente.

Em um ambiente corporativo dinâmico, a necessidade de acesso rápido e preciso a informações sobre políticas internas é crucial. Este agente foi projetado para aliviar a carga sobre as equipes de suporte, fornecendo respostas instantâneas e consistentes, 24 horas por dia, 7 dias por semana. Ele não apenas melhora a experiência do usuário final, mas também libera recursos valiosos da equipe de Service Desk para tarefas mais complexas e estratégicas.

## ✨ Destaques do Projeto

Este agente de IA é construído sobre uma arquitetura robusta e modular, incorporando as seguintes funcionalidades chave:

- **Triagem Inteligente de Intenção:** Uma das capacidades mais críticas do agente é a sua habilidade de classificar a intenção da mensagem do usuário com alta precisão. As mensagens são categorizadas em três tipos principais: `AUTO_RESOLVER` (a pergunta pode ser respondida diretamente pela base de conhecimento), `PEDIR_INFO` (requer informações adicionais do usuário antes de prosseguir) ou `ABRIR_CHAMADO` (a questão é complexa e necessita da intervenção humana de um técnico de suporte).

- **Geração Aumentada por Recuperação (RAG) Avançada:** Para as perguntas classificadas como `AUTO_RESOLVER`, o agente emprega uma técnica sofisticada de Geração Aumentada por Recuperação. Ele busca proativamente informações relevantes em uma base de conhecimento composta por documentos PDF, extrai os trechos mais pertinentes e formula uma resposta clara e concisa. O diferencial aqui é a inclusão de citações diretas e contexto dos documentos originais, garantindo a transparência e a confiabilidade das informações fornecidas.

- **Orquestração de Fluxo Dinâmica com LangGraph:** O coração da inteligência do agente reside na sua capacidade de orquestrar um fluxo de decisões complexo e adaptativo. Utilizando o **LangGraph**, o agente navega por diferentes estados e ações com base na intenção do usuário e nas informações disponíveis. Isso garante que cada interação seja guiada de forma lógica, resultando na resposta ou encaminhamento mais apropriado para cada cenário, desde a resolução automática até a escalada para um chamado humano.





## 🛠️ Pré-requisitos Essenciais

Para embarcar nesta jornada e colocar o seu Agente de Service Desk em funcionamento, você precisará dos seguintes componentes:

- **Google Colab:** Este projeto foi meticulosamente desenvolvido e otimizado para o ambiente do Google Colab, garantindo uma experiência de execução fluida e sem complicações. Embora seja possível adaptá-lo para outros ambientes, o Colab é o ponto de partida recomendado para aproveitar ao máximo suas funcionalidades.

- **Python 3.8+:** Certifique-se de ter uma versão do Python igual ou superior a 3.8 instalada. Esta versão garante a compatibilidade com todas as bibliotecas e dependências utilizadas no projeto.

- **Google API Key:** A inteligência do nosso agente é alimentada pelos modelos de linguagem do Google Gemini. Para acessá-los, você precisará de uma chave de API válida. Você pode obtê-la de forma gratuita e rápida no [Google AI Studio](https://aistudio.google.com/app/apikey). Esta chave é fundamental para a comunicação do agente com os modelos de IA.




## 🚀 Guia de Início Rápido: Colocando o Agente em Ação

Siga estes passos detalhados para configurar e executar o seu Agente de Service Desk Inteligente no Google Colab:

### 1. Configuração do Ambiente

O primeiro passo é garantir que todas as bibliotecas necessárias estejam instaladas. No seu notebook do Google Colab, execute a seguinte célula de código. Este comando instalará de forma silenciosa e atualizará todas as dependências essenciais para o funcionamento do agente:

```bash
pip install -q --upgrade langchain-google-genai langchain_community faiss-cpu langchain-text-splitters pymupdf langgraph
```

### 2. Configuração Segura da Chave da API

Para garantir a segurança da sua chave de API do Google, o projeto utiliza o recurso `userdata` do Colab. Siga as instruções abaixo para configurar sua chave:

1.  No painel esquerdo do Google Colab, localize e clique no ícone de chave (🔑).
2.  Adicione uma nova chave secreta com o nome exato `GOOGLE_API_KEY`.
3.  Cole sua chave de API do Google Gemini no campo de valor correspondente.
4.  Após adicionar a chave, execute a célula Python a seguir no seu notebook para carregar a chave de forma segura:

```python
from google.colab import userdata
from langchain_google_genai import ChatGoogleGenerativeAI

GOOGLE_API_KEY = userdata.get('GOOGLE_API_KEY')
```

### 3. Carregamento dos Documentos de Política

O agente depende de uma base de conhecimento para responder às perguntas. Para isso, você precisará carregar os documentos PDF que contêm as políticas internas da sua empresa. Siga estes passos:

1.  No painel lateral esquerdo do Colab, clique no ícone de pasta (📂) para abrir o explorador de arquivos.
2.  Clique no ícone de upload (⬆️) e faça o upload dos seus arquivos PDF para a pasta `/content/`.
    *   **Exemplos de arquivos:**
        *   `Política de Uso de E-mail e Segurança da Informação.pdf`
        *   `Política de Reembolsos (Viagens e Despesas).pdf`
        *   `Políticas de Home Office.pdf`
3.  Após o upload, execute as células subsequentes no notebook que são responsáveis por carregar, dividir e criar o banco de dados vetorial com o conteúdo desses documentos. Este processo é crucial para a funcionalidade RAG do agente.

### 4. Execução do Agente

Com todas as dependências instaladas e os documentos carregados, você está pronto para testar o agente. A função `grafo.invoke()` é o ponto de entrada principal que orquestra todo o fluxo de decisão e resposta para cada pergunta. Você pode utilizar os exemplos de mensagens já incluídos no notebook para ver o agente em ação:

```python
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
```

Este resumo consolida as funcionalidades, a configuração e a execução do projeto em um único texto para sua referência.




## ➡️ Como Levar Este Projeto para o GitHub

Para compartilhar este projeto incrível com a comunidade ou mantê-lo versionado no GitHub, a maneira mais simples, especialmente se você estiver trabalhando no Google Colab, é através da própria interface do Colab. Siga estes passos:

1.  No seu notebook do Colab, navegue até a barra de menu superior.
2.  Clique em **Arquivo**.
3.  No menu suspenso, selecione a opção **Salvar uma cópia no GitHub**.
4.  O Colab solicitará que você autorize o acesso à sua conta do GitHub, caso ainda não o tenha feito. Conceda as permissões necessárias.
5.  Após a autorização, você poderá escolher o repositório e a `branch` específicos onde deseja salvar o seu notebook.
6.  Adicione uma mensagem de `commit` descritiva para registrar suas alterações e clique em **OK**.

Pronto! Seu projeto estará no GitHub, pronto para ser compartilhado e aprimorado.




## 🤝 Contribuição e Suporte

Este projeto é um convite à inovação! Sinta-se à vontade para explorar, adaptar e aprimorar este agente de Service Desk. Contribuições são sempre bem-vindas, seja através de sugestões, relatórios de bugs ou pull requests. Juntos, podemos construir soluções de IA cada vez mais inteligentes e eficientes.

Se você tiver dúvidas, sugestões ou precisar de suporte, não hesite em abrir uma *issue* neste repositório. Sua colaboração é fundamental para o crescimento e a evolução deste projeto!

## 📄 Licença

Este projeto está licenciado sob a [Licença MIT](https://opensource.org/licenses/MIT). Sinta-se à vontade para utilizá-lo e modificá-lo conforme suas necessidades.

--- 
