# BigQuery, SQL e Apps Script para Marketing Ops | Milvus

**Foco:** preparação de bases, tratamento de dados e rotinas de integração para apoiar análises de marketing.

**Entrega:** estruturação de consultas SQL, módulos de Apps Script e documentação operacional para conectar dados do RD Station a uma base de análise.

**Status:** desenvolvimento incremental, com etapas validadas e pendências de integração identificadas.

## Contexto

Os dados de contatos, conversões e qualificação precisavam ser organizados para apoiar a leitura da operação de marketing. O desafio envolvia reunir informações do RD Station, conferir a consistência das cargas e preparar campos para consulta e visualização.

O trabalho foi desenvolvido de forma incremental: preparação de bases, testes de importação, ajustes de tipos e consultas, configuração de indicadores e registro das decisões técnicas.

## Meu papel

Atuei na definição das necessidades de marketing, organização dos campos, acompanhamento das integrações e validação das informações apresentadas nas bases e no painel.

Desenvolvi minha prática em SQL e Apps Script com apoio de IA na escrita e revisão de código. Minha participação incluiu traduzir perguntas de negócio em requisitos, aplicar e testar ajustes, investigar inconsistências e documentar o funcionamento das rotinas.

## Problema de negócio

Uma base disponível para consulta ainda precisa de critérios para ser usada em decisões. Algumas perguntas que orientaram o projeto foram:

- O total de linhas corresponde ao total de contatos únicos?
- Há campos vazios ou valores que não foram convertidos corretamente?
- A data representa uma conversão, a criação do contato ou a consulta à API?
- O contato possui uma oportunidade atual ou apenas um registro histórico?
- Quais contatos precisam de revisão antes do encaminhamento comercial?

## Estrutura trabalhada

O fluxo documentado para os dados de contatos foi:

**RD Station → Apps Script → Google Sheets → BigQuery → Looker Studio**

| Camada | Papel no projeto |
|---|---|
| RD Station | Fonte de contatos, conversões e informações de qualificação |
| Apps Script | Consulta à API, importação em lotes, atualização e diagnóstico |
| Google Sheets | Organização das bases e conferência operacional |
| BigQuery | Leitura da base, tratamento de tipos e consultas de qualidade |
| Looker Studio | Indicadores, filtros e tabelas para acompanhamento |

A importação inicial de contatos e a atualização da base de qualificação foram documentadas como rotinas distintas, com campos e finalidades próprios. A captura de eventos comerciais também foi tratada separadamente da situação atual dos contatos.

## O que foi desenvolvido

### 1. Importação e atualização com Apps Script

Foram preparados módulos para consultar contatos por paginação, trabalhar em lotes, registrar progresso e apoiar a retomada após falhas.

As rotinas incluíram conferência de UUIDs, comparação de totais, acompanhamento de status e preparação de acionadores. A documentação diferencia a conclusão de um lote da conclusão de toda a carga.

### 2. Organização dos dados no BigQuery

Foram estruturados arquivos SQL para:

- criar uma tabela externa de leitura da planilha;
- conferir quantidade de linhas, contatos distintos e preenchimento dos campos;
- criar uma visão com campos tipados;
- auditar valores preenchidos que não puderam ser convertidos.

O tratamento contemplou textos, números, datas, horários e booleanos. Valores ausentes ou inválidos foram preservados como desconhecidos quando apropriado, evitando transformar falta de informação em zero ou falso.

### 3. Consultas para análise de marketing

As consultas e os exercícios aplicados envolveram seleção de campos, filtros, agrupamentos, contagens e inspeção de esquemas.

O objetivo foi desenvolver uma base consultável para investigar origem, estágio de funil, conversões e situação comercial, respeitando os campos efetivamente disponíveis em cada fonte.

### 4. Documentação dos códigos

Os materiais foram organizados por finalidade, origem e versão, com explicações sobre:

- o que cada rotina faz;
- quais são suas entradas e saídas;
- quais funções permitem iniciar, acompanhar ou pausar o processamento;
- quais etapas dependem de validação no ambiente real;
- quais trechos são exemplos de aprendizado ou tentativas anteriores.

## Desafio técnico em destaque: consistência da carga

Uma importação apresentou diferença entre a quantidade de linhas lidas e o total de UUIDs únicos. A rotina bloqueou a publicação da carga inconsistente.

A investigação levou à revisão da ordenação da consulta e à manutenção de verificações antes da publicação. Esse episódio reforçou uma decisão central do projeto: **conferir a base antes de usá-la no dashboard**.

A repetição de UUIDs na leitura não foi tratada como prova de cadastros duplicados no RD Station. A base pode mudar durante a paginação, e a causa precisava ser investigada.

## Decisões de qualidade dos dados

| Decisão | Por que importa |
|---|---|
| Comparar linhas e contatos distintos | Ajuda a identificar repetições e divergências na carga |
| Auditar conversões de tipos | Evita que erros de formato passem despercebidos |
| Preservar nulos | Mantém a diferença entre ausência de informação e resposta negativa |
| Distinguir consulta à API de atividade do lead | Evita interpretar atualização técnica como intenção comercial |
| Separar oportunidade histórica e situação atual | Apoia uma leitura mais cuidadosa para priorização |
| Documentar versões e testes | Facilita manutenção, retomada e revisão dos ajustes |

## Entregas e validação

| Entrega | Evidência disponível e estágio |
|---|---|
| Módulos de Apps Script | Arquivos de importação, atualização, diagnóstico e configuração recuperados |
| Estrutura SQL | Arquivos de criação da tabela externa, visão tipada e consultas de conferência |
| Documentação operacional | Manual, catálogo de códigos e lista de pendências organizados |
| Filtros e acesso aos contatos no painel | Funcionamento confirmado durante os testes registrados no histórico |
| Captura ampliada de conversões | Recebimento inicial registrado; cobertura geral ainda dependia de validação |
| Atualização completa e contínua | Exige conferir a versão instalada, os acionadores e a inclusão de novos contatos |

As entregas demonstram evolução na organização e validação da base analítica. Não foram medidos neste case ganho de receita, redução de custos ou economia de horas atribuíveis às rotinas.

## Aprendizados

- O código precisa responder a uma pergunta de negócio e ter critérios de conferência.
- Uma execução sem erro não significa que toda a base foi atualizada.
- Tipos, nulos e significado das datas alteram a interpretação dos indicadores.
- Documentar o funcionamento ajuda a explicar, manter e revisar uma integração.
- SQL e Apps Script ampliaram minha capacidade de investigar dados e apoiar a operação de marketing.

## Próximos passos

- Conferir os objetos existentes no BigQuery e as versões instaladas dos scripts.
- Validar a atualização completa e a inclusão de novos contatos.
- Concluir os testes de cobertura da captura de conversões.
- Medir duração, falhas e completude das cargas ao longo do tempo.
- Evoluir as análises de qualidade dos leads e encaminhamento comercial.

---

*Case baseado em atividades reais na Milvus. A apresentação resume a arquitetura e as decisões do projeto, sem expor bases de contatos, credenciais ou identificadores internos. O trabalho permanece em evolução.*

