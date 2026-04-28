# -Atividade3_TTC_Rafael_-iza--O-
codigo lora 

# Relatório Técnico: Automação de Auditoria de Patrimônio via Power Automate

## 1. Introdução e Visão Geral
Este relatório detalha a implementação de um ecossistema de automação desenvolvido para otimizar o processo de auditoria de ativos (computadores e smartphones). A solução utiliza o Microsoft Power Automate para substituir processos manuais por fluxos sistêmicos, garantindo escalabilidade, padronização e rastreabilidade.

### 1.1 Conceito Base: Estrutura de um Fluxo de Automação
Um fluxo no Microsoft Power Automate é uma sequência lógica de instruções construída sobre três componentes estruturais básicos:

* **Gatilhos (Triggers):** Representam o evento inicializador que dispara a execução do script (manual ou agendado).
* **Ações (Actions):** Operações sistêmicas como conectar a bases de dados (Excel) e disparar mensagens via Microsoft Teams.
* **Controles Lógicos (Conditions):** Regras de negócio, laços de repetição e estruturas condicionais para tomada de decisão em tempo real.

---

## 2. Metodologia e Arquitetura Lógica
A solução foi desenhada com base em uma arquitetura modular, dividida em quatro fluxos principais para garantir a estabilidade e facilitar manutenções futuras.

### 2.2.2.1 Arquitetura do Fluxo de Envio (Orquestrador 1)
O fluxo de envio opera de forma linear, processando a base de dados e integrando-se nativamente ao Microsoft Teams.

1.  **Inicialização:** Acionamento manual via gatilho.
2.  **Extração de Dados:** Ação "Listar linhas presentes em uma tabela" para recuperar registros do inventário.
3.  **Processamento Iterativo:** Laço "Aplicar a cada" para processamento individual.
4.  **Integração com o Teams:** Ações "Criar um chat" e "Postar mensagem em um chat ou canal" com texto parametrizado.
5.  **Controle de Cadência:** Instrução de "Atraso" para mitigação de *rate limiting*.

### 2.2.2.2 Arquitetura do Fluxo de Captura de Identificadores (IDs)
Responsável por extrair os identificadores únicos de cada chat no Microsoft Teams e gravá-los na base de controle para rastreabilidade.

1.  **Requisição via Microsoft Graph API:** Consulta de baixo nível para extração de metadados complexos.
2.  **Tratamento do Payload:** Operações de "Matriz do filtro" e "Compor" para *parsing* de dados JSON.
3.  **Mapeamento de Sessões:** Bloco "Listar chats" para consulta de canais ativos.
4.  **Persistência e Cadência:** Gravação do ID na tabela e controle de *throughput* via atraso.

### 2.2.2.3 Arquitetura do Fluxo de Captura de Respostas
Fluxo de retorno (*inbound*) projetado para ler o histórico de comunicações e atualizar a base de forma autônoma.

1.  **Ingestão de Dados Pendentes:** Leitura de registros aguardando validação e seus IDs de chat.
2.  **Extração de Histórico:** Ação "Get messages in a chat" para importar a conversa integral.
3.  **Processamento e Parsing:** Expurgo de metadados sistêmicos e isolamento do conteúdo textual das interações.
4.  **Validação de Identidade:** Uso do "Obter perfil do usuário (V2)" para assegurar a autenticidade do remetente.
5.  **Persistência:** Atualização da linha na base de dados mediante conformidade lógica.

### 2.2.2.4 Arquitetura do Fluxo de Análise e Classificação (IA)
Integra capacidades de Inteligência Artificial para interpretar as respostas e automatizar o veredito da auditoria.

1.  **Recuperação de Dados:** Extração de blocos de texto brutos das interações dos usuários.
2.  **Processamento de Linguagem Natural (NLP):** Uso de modelos cognitivos para determinar se a resposta configura confirmação, recusa ou exceção.
3.  **Atualização de Status:** Consolidação do resultado final na base de dados sem intervenção humana.

---

## 3. Aplicações Práticas e Limitações Técnicas

### 3.1 Estudos de Caso e Aplicações Operacionais
* **Auditoria Integral (100%):** Expansão de uma amostragem de 5% para cobertura total de 1.004 máquinas.
* **Expansão para Dispositivos Móveis:** Escalonamento da lógica para smartphones corporativos.
* **Canal Padronizado:** Estabelecimento de um canal oficial de baixo custo para comunicados de TI.

### 3.2 Limitações Técnicas e Restrições de Sistema
* **Rate Limiting:** Restrição de ~100 usuários por lote imposta pelo Microsoft Teams (Erro 429).
* **Assincronicidade:** Dependência da latência de resposta do fator humano.
* **Precisão do Modelo Semântico:** Sujeição a variações de linguagem natural e ambiguidades.

---

## 4. Conclusão e Trabalhos Futuros

### 4.1 Considerações sobre a Implementação e Prototipagem
Os fluxos detalhados representam o modelo **ideal**. Diferenças em relação ao protótipo inicial justificam-se pela aplicação de parâmetros de segurança (atrasos e paginação) necessários para o ambiente produtivo.

### 4.2 Propostas de Melhorias Futuras
* **Migração para SharePoint Lists ou Dataverse:** Para maior robustez e integridade de dados.
* **Refinamento de Prompts de IA:** Para aumentar a acurácia da classificação semântica.
* **Dashboard em Power BI:** Para visualização em tempo real da taxa de conformidade.
* **Adaptive Cards:** Para padronização absoluta da resposta do usuário via botões interativos.
