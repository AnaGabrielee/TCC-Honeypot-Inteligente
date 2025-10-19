# TCC – Honeypots Inteligentes com Modelos de Linguagem para Contenção e Análise de Ameaças na Nuvem

## 1. Introdução
Este projeto integra o Trabalho de Conclusão de Curso (TCC) desenvolvido no âmbito do curso de Engenharia da Computação, com foco na área de Cibersegurança e Computação em Nuvem. O trabalho propõe a criação de um honeypot inteligente baseado em Modelos de Linguagem de Grande Escala (LLMs), com o objetivo de aprimorar a detecção, análise e contenção de ameaças em ambientes cloud-native.

Diferentemente de honeypots tradicionais, que operam de forma estática e limitada, o sistema aqui desenvolvido utiliza um modelo de linguagem para gerar respostas dinâmicas e contextualmente coerentes, simulando um ambiente de terminal realista. Essa abordagem permite manter o atacante engajado por mais tempo, coletando dados de maior valor analítico e viabilizando ações automatizadas de resposta a incidentes em nuvem.

O projeto busca unir técnicas de defesa cibernética com inteligência artificial generativa, explorando o potencial de LLMs na criação de ambientes interativos, capazes de produzir informações detalhadas sobre o comportamento do atacante e suas Táticas, Técnicas e Procedimentos (TTPs).

## 2. Objetivos

### 2.1 Objetivo Geral
Desenvolver uma Prova de Conceito (PoC) de um honeypot inteligente capaz de empregar Modelos de Linguagem de Grande Escala (LLMs) para simular um ambiente interativo, realizar coleta estruturada de dados de ataque e permitir contenção automatizada em ambientes de nuvem.

### 2.2 Objetivos Específicos
- Criar um servidor honeypot em Python utilizando sockets TCP, simulando um terminal Linux vulnerável.  
- Integrar o servidor com a API de um modelo de linguagem (Gemini) para geração de respostas coerentes e contextuais.  
- Implementar um sistema de gestão de contexto capaz de manter o histórico da sessão do atacante.  
- Estruturar logs em formato JSON Lines (.jsonl) para registrar IP, timestamp, comando e resposta.  
- Mapear os dados coletados de acordo com o framework MITRE ATT&CK®.  
- Desenvolver uma arquitetura de implantação em nuvem utilizando AWS EC2, CloudWatch e Lambda.  
- Comparar o desempenho do honeypot inteligente com o de um honeypot tradicional de média interação, como o Cowrie.

## 3. Arquitetura da Solução

### 3.1 Arquitetura Local (Protótipo)
O ambiente de desenvolvimento é composto por duas máquinas: uma servidora e uma atacante. O servidor Ubuntu Server 22.04 LTS executa o honeypot, enquanto o Kali Linux, configurado em máquina virtual (modo bridge), é utilizado para a simulação de ataques.

Fluxo de interação:

Atacante (Kali Linux) ⇄ Servidor Honeypot (Ubuntu Server) ⇄ Módulo de Contexto e Prompt ⇄ API do LLM (Gemini)

**Componentes principais:**
1. Servidor Honeypot (`honeypot.py`): servidor TCP que recebe e processa conexões.  
2. Módulo de Contexto: gerencia o histórico de comandos e simula o estado do sistema de arquivos.  
3. Integração com Gemini: realiza requisições à API e gera respostas contextuais.  
4. Sistema de Logs: armazena interações em formato estruturado (.jsonl) para análise posterior.

O desenvolvimento é conduzido utilizando o Visual Studio Code com acesso remoto via SSH, assegurando controle e eficiência no ambiente de trabalho.

### 3.2 Arquitetura de Implantação em Nuvem (Proposta)
A arquitetura proposta para implantação em nuvem baseia-se na Amazon Web Services (AWS), aproveitando seus serviços nativos para monitoramento e automação da resposta a incidentes.

| Componente | Descrição |
|-------------|------------|
| Amazon EC2 | Instância que hospeda o honeypot. |
| Amazon CloudWatch | Serviço responsável por centralizar e monitorar logs de interação. |
| AWS Lambda | Função acionada por eventos no CloudWatch para modificar regras de segurança (Security Groups) e bloquear endereços IP maliciosos. |
| Security Groups | Mecanismo de controle de tráfego e firewall da instância EC2, configurável de forma programática. |

Essa estrutura permite a execução de ações de contenção automatizadas e adaptativas, representando um avanço prático em relação a soluções tradicionais de defesa.

## 4. Metodologia de Pesquisa e Avaliação

### 4.1 Coleta e Mapeamento de TTPs
Os dados são coletados a partir dos logs gerados pelo honeypot, contendo timestamp, endereço IP, comando e resposta. Cada sessão é analisada individualmente para identificar padrões de comportamento e correlacioná-los com as Táticas, Técnicas e Procedimentos (TTPs) do framework MITRE ATT&CK®.

Exemplo:
Comandos como `whoami`, `ls -la` e `cat /etc/passwd` são mapeados às táticas de Discovery (TA0007) e Credential Access (TA0006).

### 4.2 Métricas de Avaliação
A eficácia do honeypot será medida com base em três critérios principais:

| Métrica | Tipo | Descrição |
|----------|------|-----------|
| Realismo das respostas | Qualitativa | Avalia se as respostas simulam corretamente o comportamento de um sistema real. |
| Tempo de engajamento | Quantitativa | Mede o tempo médio de interação do atacante com o honeypot. |
| Profundidade de TTPs | Quantitativa | Analisa a variedade e complexidade das ações identificadas nos logs. |

### 4.3 Análise Comparativa
Os resultados obtidos com o honeypot inteligente serão comparados aos do honeypot tradicional Cowrie, implantado sob as mesmas condições de rede e exposição. Essa comparação visa demonstrar, com base em dados empíricos, os benefícios do uso de LLMs na ampliação da eficácia e realismo das interações.

## 5. Ferramentas e Tecnologias Utilizadas

| Categoria | Ferramenta/Tecnologia | Função |
|------------|----------------------|--------|
| Linguagem | Python 3.10+ | Desenvolvimento do servidor honeypot |
| Rede | socket (Python) | Criação de comunicação TCP entre servidor e atacante |
| Inteligência Artificial | Google Generative AI (Gemini) | Geração de respostas realistas e contextuais |
| Sistema Operacional | Ubuntu Server 22.04 LTS | Servidor honeypot |
| Testes de Ataque | Kali Linux (VirtualBox) | Simulação de ataques controlados |
| Nuvem (Proposta) | AWS EC2, CloudWatch, Lambda | Implantação e automação da contenção |
| IDE | Visual Studio Code + SSH Remote | Desenvolvimento remoto |
| Versionamento | Git e GitHub | Controle de versões e documentação |

## 6. Estrutura do Repositório

```
TCC-Honeypot-Inteligente/
├── src/
│   ├── honeypot.py             # Servidor principal
│   ├── prompt_manager.py       # Módulo de contexto
│   ├── logger.py               # Logs estruturados (.jsonl)
│   └── ...
├── docs/
│   ├── Relatorios/             # Relatórios técnicos e acadêmicos
│   ├── Diagramas/              # Diagramas de arquitetura e fluxos
│   ├── Planejamento/           # Cronogramas e melhorias
│   └── ...
├── research/
│   ├── artigos/                # Artigos e referências
│   └── estudos/                # Análises e frameworks
├── requirements.txt            # Dependências Python
├── README.md                   # Documento principal
└── .gitignore
```

## 7. Etapas do Projeto

**Fase 1 – Configuração e Estruturação (Concluída)**  
Configuração do ambiente (Ubuntu e Kali), criação da versão inicial do servidor (v1.0) e testes de conectividade.

**Fase 2 – Integração e Inteligência (Concluída)**  
Integração com o modelo Gemini, implementação da arquitetura híbrida (Python + LLM) e aprimoramento dos logs.

**Fase 3 – Testes e Coleta de Dados (Em andamento)**  
Execução de testes com ataques simulados, coleta e análise de logs e mapeamento de TTPs segundo o MITRE ATT&CK®.

**Fase 4 – Implantação e Análise Comparativa (Próxima)**  
Integração do honeypot à arquitetura AWS e comparação de desempenho com o honeypot Cowrie.

## 8. Equipe e Responsabilidades

| Membro | Função | Responsabilidades |
|--------|---------|-------------------|
| Ana Gabriele Costa Viana (@anagabrielee) | Desenvolvimento e Implementação | Responsável pela parte prática do projeto, desenvolvimento do honeypot em Python, integração com o modelo Gemini, testes e validação. |
| Elion Santiago Feitoza (@elionsantiago) | Documentação e Análise de Dados | Responsável pela documentação técnica e acadêmica, análise dos logs gerados e fundamentação teórica do projeto. |

## 9. Licença e Observações Finais
Este projeto é de caráter estritamente acadêmico, desenvolvido no contexto de Trabalho de Conclusão de Curso, sem fins comerciais. As informações e códigos apresentados destinam-se a fins de pesquisa e ensino.

O projeto demonstra a viabilidade de integrar Modelos de Linguagem de Grande Escala a honeypots de média interação, proporcionando um ambiente de defesa mais realista, analítico e automatizado. Os resultados obtidos até o momento indicam potencial significativo para aplicação prática e para futuras pesquisas em segurança cibernética e defesa adaptativa.

**Última atualização:** Outubro de 2025  
**Versão atual do protótipo:** 4.5
