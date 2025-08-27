## 1. Introdução

Este projeto de Trabalho de Conclusão de Curso (TCC) foca no desenvolvimento de uma Prova de Conceito (PoC) de um **honeypot inteligente customizado**. Diferente de soluções tradicionais, este sistema utilizará um Modelo de Linguagem de Grande Escala (LLM), como a API do Gemini, para gerar interações dinâmicas e realistas com o objetivo de engajar atacantes, coletar dados detalhados sobre suas Táticas, Técnicas e Procedimentos (TTPs) e testar mecanismos de contenção em ambientes de nuvem.

## 2. Objetivos

* **Desenvolver um Protótipo de Honeypot Inteligente:** Criar um servidor customizado em Python que simula um terminal de comandos (shell) vulnerável, utilizando a biblioteca `socket`.
* **Integrar com um Modelo de Linguagem:** Utilizar a API de um LLM (Gemini) para processar os comandos recebidos e gerar respostas falsas, coerentes e que mantenham o contexto da sessão do atacante.
* **Analisar e Mapear Táticas, Técnicas e Procedimentos (TTPs):** Coletar os dados de interação e mapeá-los de acordo com o framework **MITRE ATT&CK®**, permitindo uma análise padronizada do comportamento do atacante.
* **Avaliar a Eficácia do Protótipo:** Medir o desempenho do honeypot com base em métricas predefinidas de realismo, tempo de engajamento e profundidade dos TTPs capturados.
* **Validar a Abordagem através de Análise Comparativa:** Comparar as métricas de eficácia do nosso protótipo com as de um honeypot tradicional de média interação (Cowrie) para validar a hipótese de que a abordagem com LLM oferece vantagens significativas.

## 3. Arquitetura da Solução

A solução é dividida em duas arquiteturas: o protótipo que será desenvolvido e testado localmente, e a arquitetura final proposta para um ambiente de produção na nuvem.

### 3.1. Arquitetura do Protótipo (Desenvolvimento Local)

O ambiente de desenvolvimento consiste em um loop de interação entre a máquina do atacante e o servidor honeypot, com a inteligência centralizada na chamada ao LLM.

`[Atacante (Kali Linux)] <--> [Servidor Honeypot (Ubuntu Server)] <--> [Módulo de Prompt e Contexto] <--> [API do LLM (Gemini)]`

### 3.2. Arquitetura de Implantação na Nuvem (Proposta)

A arquitetura final foi projetada para a AWS, utilizando serviços "Cloud-native" para automação, monitoramento e resposta a incidentes.

* **AWS EC2:** Instância para executar o script Python do honeypot.
* **Amazon CloudWatch:** Serviço para centralizar e monitorar os logs de interação gerados.
* **AWS Lambda:** Função "serverless" acionada por alertas no CloudWatch para executar ações de contenção.
* **Security Groups:** Atuam como firewall, com regras que podem ser modificadas programaticamente pela função Lambda para bloquear o IP do atacante.

## 4. Tecnologias e Ferramentas

| Categoria          | Ferramenta/Tecnologia         | Justificativa                                                     |
| :----------------- | :---------------------------- | :---------------------------------------------------------------- |
| **Desenvolvimento** | Python 3.10+                  | Linguagem principal para o desenvolvimento do servidor honeypot.    |
|                    | Biblioteca `socket`           | Para criação do servidor TCP de baixo nível que simula o terminal.|
|                    | Biblioteca `google-generativeai` | Para integração com a API do LLM Gemini.                         |
| **Ambiente** | Ubuntu Server 22.04 LTS       | Sistema operacional base onde o honeypot será executado.          |
|                    | Kali Linux                    | Sistema operacional para simular os ataques e testar a eficácia.   |
| **Cloud (Proposta)** | AWS (EC2, CloudWatch, Lambda) | Provedor de nuvem para a arquitetura de implantação final.         |
| **Ferramentas Auxiliares**| Git & GitHub                  | Para versionamento de código e documentação.                      |
|                    | Visual Studio Code (com SSH)  | IDE para desenvolvimento remoto e eficiente no servidor Ubuntu.   |

## 5. Metodologia de Análise e Avaliação

Para transformar os dados brutos coletados em inteligência acionável e avaliar a eficácia do protótipo, a seguinte metodologia será empregada:

### 5.1. Coleta e Análise de TTPs

A análise das Táticas, Técnicas e Procedimentos (TTPs) seguirá um processo estruturado:

1.  **Fonte de Dados:** O arquivo `honeypot.log`, que captura de forma estruturada o `timestamp`, `IP do atacante`, `comando enviado` e a `resposta gerada pelo LLM`.
2.  **Processamento:** Os logs serão processados para extrair a sequência de comandos de cada sessão de ataque.
3.  **Mapeamento com o Framework MITRE ATT&CK®:** Os comandos e suas sequências serão categorizados e mapeados para as táticas e técnicas correspondentes do framework MITRE ATT&CK®.
    * **Exemplo:** Uma sequência de comandos como `whoami`, `ls -la`, `cat /etc/passwd` será mapeada para a tática de **Discovery (TA0007)** e **Credential Access (TA0006)**.
4.  **Objetivo da Análise:** O objetivo é ir além da simples listagem de comandos, buscando identificar o **comportamento, a intenção e a estratégia** do atacante.

### 5.2. Critérios de Avaliação de Eficácia

A eficácia do honeypot inteligente será avaliada com base em um conjunto de métricas qualitativas e quantitativas:

* **Métrica 1: Realismo e Coerência das Respostas (Qualitativa):**
    * **Descrição:** Análise da qualidade das respostas geradas pelo LLM.
    * **Critérios:** As respostas simulam corretamente os outputs de um shell Linux? O sistema mantém o contexto da sessão de forma convincente?

* **Métrica 2: Tempo de Engajamento do Atacante (Quantitativa):**
    * **Descrição:** Medir por quanto tempo o honeypot consegue manter o interesse do atacante.
    * **KPIs (Key Performance Indicators):** Duração média da sessão por atacante (em minutos) e número médio de comandos por sessão.

* **Métrica 3: Profundidade e Variedade dos TTPs Capturados (Quantitativa):**
    * **Descrição:** Avaliar a complexidade das interações que o honeypot consegue provocar.
    * **KPIs:** Número de táticas distintas do MITRE ATT&CK® que foram identificadas durante os testes.

### 5.3. Análise Comparativa com Honeypots Tradicionais

Para validar cientificamente os benefícios da abordagem com LLM, os resultados do nosso protótipo serão comparados com um honeypot de média interação estabelecido, como o **Cowrie**.

1.  **Ambiente Controlado:** Ambos os honeypots (o nosso e o Cowrie) serão implantados em ambientes idênticos e expostos aos mesmos testes de ataque.
2.  **Comparação de Métricas:** As métricas de **Tempo de Engajamento** e **Profundidade de TTPs** serão coletadas para ambos os sistemas.
3.  **Validação da Hipótese:** O objetivo é demonstrar, com dados, se o honeypot inteligente com LLM é capaz de gerar sessões de ataque mais longas e/ou capturar TTPs mais complexos.

## 6. Estrutura do Repositório

TCC-Honeypot-Inteligente/
│
├── src/                 # Código fonte do projeto
│   └── honeypot.py      # Script principal do honeypot
│
├── docs/                # Documentação do projeto, diagramas de arquitetura
├── research/            # Artigos e pesquisas de referência
├── .gitignore           # Arquivo para ignorar arquivos desnecessários (ex: venv/)
├── README.md            # Este arquivo
└── requirements.txt     # Lista de dependências Python do projeto


## 7. Como Executar (v1 - Protótipo Básico)

Os passos abaixo descrevem como executar a primeira versão do protótipo.

1.  **Pré-requisitos:**
    * Um servidor com Ubuntu Server 22.04 LTS.
    * Python 3.10+ e Git instalados.

2.  **Instalação:**
    ```bash
    # 1. Clone o repositório
    git clone [https://github.com/AnaGabrielee/TCC-Honeypot-Inteligente.git](https://github.com/AnaGabrielee/TCC-Honeypot-Inteligente.git)

    # 2. Acesse a pasta do projeto
    cd TCC-Honeypot-Inteligente

    # 3. Crie e ative um ambiente virtual
    python3 -m venv venv
    source venv/bin/activate
    ```

3.  **Execução:**
    ```bash
    # Execute o script do honeypot
    python src/honeypot.py
    ```
    O servidor deverá exibir a mensagem: `[*] Honeypot escutando em 0.0.0.0:2222...`

4.  **Teste (a partir da máquina Kali Linux):**
    ```bash
    # Use o netcat para se conectar ao honeypot
    nc <IP_DO_SERVIDOR_UBUNTU> 2222
    ```

## 8. Roadmap do Projeto

* [x] **Fase 1: Fundação e Estrutura**
    * [x] Definição da arquitetura e tecnologias.
    * [x] Configuração do ambiente (Ubuntu Server, Kali VM).
    * [x] Criação da v1 do servidor honeypot com `socket`.
    * [x] Estruturação do repositório no GitHub.

* [ ] **Fase 2: Desenvolvimento do Protótipo Inteligente**
    * [ ] Obtenção da chave de API do Gemini.
    * [ ] Integração da API do LLM para gerar respostas dinâmicas.
    * [ ] Implementação da gestão de contexto (`session_history`).

* [ ] **Fase 3: Testes e Coleta de Dados**
    * [ ] Execução de ataques simulados a partir do Kali Linux.
    * [ ] Coleta e análise dos logs para mapeamento MITRE ATT&CK®.
    * [ ] Início dos testes comparativos com o Cowrie.

* [ ] **Fase 4: Finalização**
    * [ ] Detalhamento da arquitetura de nuvem proposta.
    * [ ] Redação e formatação do trabalho acadêmico final.
    * [ ] Preparação da apresentação para a banca.

## 9. Equipe e Divisão de Atividades

O projeto é desenvolvido pela seguinte equipe:

* **Ana Gabriele Costa Viana** (@anagabrielee)
    * **Foco:** Desenvolvimento do script principal do honeypot em Python.
    * **Atividades:** Integração com a API do LLM, engenharia de prompts, gestão de contexto da sessão e documentação técnica do código.

* **Elion Santiago Feitoza** (@elionsantiago)
    * **Foco:** Análise de Dados e Inteligência de Ameaças.
    * **Atividades:** Pesquisa de TTPs, análise dos logs gerados, mapeamento para o framework MITRE ATT&CK®, e análise comparativa dos resultados.

* **Raniel da Silva Nascimento** (@raniel22)
    * **Foco:** Arquitetura e Infraestrutura.
    * **Atividades:** Configuração do ambiente (Ubuntu Server, Kali, Cowrie), definição da arquitetura de nuvem (AWS) e planejamento dos testes de integração.

## 10. Licença

Este projeto é de caráter acadêmico, desenvolvido no contexto de Trabalho de Conclusão de Curso, e não possui fins comerciais ou distribuição pública.

Este projeto é de caráter acadêmico, desenvolvido no contexto de Trabalho de Conclusão de Curso, e não possui fins comerciais ou distribuição pública.
