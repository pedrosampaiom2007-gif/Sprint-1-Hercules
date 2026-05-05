# Sprint-1-Hercules

# EV ChargeOps — Chatbot Inteligente

> Assistente conversacional com IA para gestão de eletropostos em condomínios,
> integrado ao ecossistema GoodWe SEMS no contexto do EV Challenge 2026.

---

## Equipe

| Nome | RM |
|------|----|
| Luan de Araujo Carneiro | RM 573691 |
| Pedro Sampaio Mochnacs Arruda | RM 573522 |
| Raul Sampaio Mochnacs Arruda | RM 573523 |
| Lucas Garcia de Britto | RM 571768 |
| Kevin Rodrigues de Melo | RM 571777 |

---

## Problema Abordado

A expansão dos veículos elétricos em condomínios residenciais expõe dois problemas
centrais que ainda carecem de solução integrada:

- **Ausência de orquestração de potência:** sem controle inteligente, o uso
simultâneo de múltiplos carregadores sobrecarrega o sistema elétrico do prédio.
- **Falta de transparência no consumo:** sem medição individual, o rateio de
custos é injusto e gera conflitos entre moradores.

Além disso, síndicos e moradores não dispõem de uma interface simples para
consultar dados de consumo, sessões de recarga ou alertas do sistema — 
dependendo de dashboards técnicos que exigem conhecimento especializado.

---

## Proposta do Chatbot

O chatbot do EV ChargeOps é um assistente conversacional com IA projetado como
ferramenta operacional real, não como demonstração genérica. Ele atua como
interface de linguagem natural entre os usuários e os dados do sistema de
gestão de eletropostos.

### Persona Atendida

O chatbot é direcionado primariamente ao **síndico** e secundariamente aos
**moradores**, respondendo perguntas sobre consumo, sessões de recarga,
alertas de demanda e cobrança — sem necessidade de acesso ao dashboard técnico.

### O que o Chatbot Responde

- Consultas de consumo por unidade e por período
- Status dos carregadores em tempo real
- Alertas e previsões de pico de demanda
- Informações sobre sessões de recarga registradas
- Dúvidas sobre cobrança e rateio de energia
- Orientações sobre o uso do sistema

---

## Tecnologias Selecionadas e Justificativa Técnica

| Tecnologia | Função | Justificativa |
|------------|--------|---------------|
| **OpenAI API (GPT-4o)** | Modelo de linguagem principal | Alta capacidade de compreensão contextual, ideal para respostas precisas em domínio técnico específico |
| **LangChain** | Orquestração do fluxo do chatbot | Facilita a injeção de contexto, gerenciamento de histórico de conversa e integração com fontes de dados externas |
| **Python + Flask** | Backend da API do chatbot | Consistência com o restante da stack do projeto, leveza e facilidade de integração |
| **API GoodWe SEMS** | Fonte de dados em tempo real | Fornece dados de geração solar, consumo e status dos carregadores diretamente ao contexto do chatbot |
| **PostgreSQL** | Histórico de sessões e consumo | Permite ao chatbot consultar dados históricos para responder perguntas de períodos específicos |

---

## Fluxo de Funcionamento

FLuxograma:

## Fluxograma

```mermaid
graph TD

    A[Usuário<br/>(Síndico ou Morador)]
    B[Backend<br/>Python + Flask]
    C{LangChain<br/>Orquestrador}

    D[System Prompt<br/>Regras GoodWe]
    E{Fontes de Dados}
    F[(API GoodWe SEMS)]
    G[(PostgreSQL<br/>Histórico de Consumo)]

    H[Pacote de Contexto]
    I[OpenAI GPT-4o]
    J[Resposta Contextualizada]

    A -->|Pergunta via Interface| B
    B --> C

    C -->|Injeta regras| D
    C -->|Busca dados| E

    E --> F
    E --> G

    F --> H
    G --> H
    D --> H

    H --> I
    I -->|Resposta natural| J
    J -->|Retorno via API| A
```

Resumo do fluxo:

1. Usuário envia uma pergunta via interface (Dashboard ou app)
2. A mensagem é recebida pelo backend Flask
3. LangChain injeta o system prompt com o contexto do EV ChargeOps
4. Se necessário, o sistema consulta a API GoodWe SEMS ou o banco de dados
5. O modelo GPT-4o processa a pergunta com o contexto completo
6. A resposta é gerada em linguagem natural e retornada ao usuário

---

## Modelo de Teste — Perguntas e Respostas Esperadas

| # | Pergunta | Resposta Esperada |
|---|----------|-------------------|
| 1 | Qual apartamento mais consumiu energia este mês? | O sistema identifica a unidade com maior consumo registrado no período e informa o valor em kWh e o custo correspondente. |
| 2 | Quantas sessões de recarga foram realizadas hoje? | O chatbot consulta o banco de dados e retorna o número de sessões do dia, com horários de início e fim de cada uma. |
| 3 | Tem algum carregador com problema agora? | O chatbot verifica o status em tempo real via API GoodWe SEMS e informa se há carregadores offline ou com falha. |
| 4 | O sistema está usando energia solar agora? | O chatbot consulta a geração solar atual e informa se há excedente sendo direcionado para os carregadores. |
| 5 | Como é feito o rateio da conta de luz entre os moradores? | O chatbot explica o modelo de medição individual por sessão, com base nas regras configuradas e na Resolução ANEEL 1.000/2021. |

---

## System Prompt Base

Você é o assistente inteligente do EV ChargeOps, um sistema de gestão de
eletropostos para condomínios residenciais integrado à plataforma GoodWe SEMS.
Seu papel é responder perguntas de síndicos e moradores sobre consumo de energia,
sessões de recarga de veículos elétricos, status dos carregadores, alertas de
demanda e cobrança individual.
Sempre responda de forma clara, objetiva e em linguagem acessível, sem jargões
técnicos desnecessários. Quando os dados forem provenientes do sistema em tempo
real, indique isso na resposta. Quando não houver dados disponíveis, informe ao
usuário e oriente como obtê-los.
Contexto do sistema:

O condomínio utiliza painéis solares integrados via API GoodWe SEMS
O sistema prioriza o uso de energia solar excedente para recarga dos veículos
O rateio de custos é individual, por sessão, seguindo a Resolução ANEEL 1.000/2021
Os carregadores são monitorados a cada 30 segundos
Picos de demanda são previstos por um modelo de Machine Learning

Nunca invente dados. Se não tiver acesso a uma informação em tempo real,
deixe claro que a consulta precisa ser feita diretamente no dashboard.
