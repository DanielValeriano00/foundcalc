# Plano de Iteração – FundCalc (Agosto‑Outubro 2025)

## Contexto

O documento de arquitetura do FundCalc define uma aplicação **SaaS** de
cálculo de fundações com arquitetura hexagona. Essa arquitetura
separa o sistema em quatro _bounded contexts_ (Negócio, Cálculo, Análise
Técnica e Sincronização) para reduzir o acoplamento entre domínios e
facilitar a evolução independente de cada módulo. Cada
contexto possui portas de entrada(APIs HTTP, eventos, webhooks,
gRPC) e portas de saída (persistência, mensageria) com separação de
comandos e consultas via *QRS. O objetivo principal
é manter o cálculo desacoplado, modularizar por domínio e garantir
integridade e auditoria】.  

Com base nessas diretrizes, este plano propõe uma iteração de cerca de
60 dias (16 de agosto a 15 de outubro de 2025) para implementar os
principais módulos e preparar a entrega da arquitetura.

## Objetivos da iteração

- Implementar os quatro contextos principais: Negócio/Core,
  Cálculo, Análise Técnica e Sincronização/Orquestração, conforme
  descritos no documento.  
- Definir e criar as portas (APIs/events) necessárias para
  comunicação entre módulos, seguindo a abordagem de Ports & Adapters e
  CQRS.  
- Desenvolver um protótipo funcional do aplicativo local(SQLite) com
  suporte offline e sincronização de eventos】.  
- Garantir rastreabilidade e auditoria, registrando eventos e
  gerenciando quotas de uso
- Gerar documentação (diagrama de componentes, modelos de dados e
  relatório técnico).

## Itens de trabalho e cronograma

A tabela a seguir apresenta os itens de trabalho, datas previstas e
responsáveis. As datas são estimativas; ajustes podem ser feitos
durante a execução.

| Nº | Período (2025) | Contexto/módulo | Tarefas principais (palavras‑chave) | Responsável |
|---|---|---|---|---|
| 1 | 16 – 18 ago | Planejamento | configurar repositório GitHub, criar backlog e definir papéis | Gerente de Projeto |
| 2 | 19 – 22 ago | Domínio | modelar entidades de Negócio (usuário, licença, assinatura, pagamento), definir _bounded contexts_, elaborar diagramas UML | GP, Dev Back‑end |
| 3 | 23 – 29 ago | Negócio/Core | implementar API HTTP de cadastro/login, licenciamento e cobrança; integrar webhooks de pagamento; persistência via JPA | Dev Back‑end |
| 4 | 30 ago – 6 set | Cálculo | desenvolver motor de cálculo determinístico de sapatas e estacas; normalização de unidades; API local (`localhostCalculator`) | Dev Cálculo |
| 5 | 7 – 13 set | Análise Técnica | implementar regras normativas (NBRs), compor parecer técnico e gerar PDF】; armazenar relatórios; emitir evento `RelatorioGerado` | Dev Análise |
| 6 | 14 – 20 set | Sincronização | construir módulo `api_sync_process` para gestão de eventos e sincronização offline‐first; implementar padrões outbox/inbox; resolver conflitos por agregado | Dev Back‑end |
| 7 | 21 – 27 set | Integração | conectar portas de entrada/saída (HTTP, gRPC, mensageria) entre os contextos; separar comandos/consultas (CQRS) | Dev Back‑end & Dev Cálculo |
| 8 | 28 set – 4 out | Aplicativo local | desenvolver protótipo da interface local (desktop/mobile) com banco SQLite【22939205343549†L95-L104】; integrar motor de cálculo local; permitir funcionamento offline | Dev Front‑end |
| 9 | 5 – 10 out | Testes e validação | elaborar testes unitários e de integração para cada módulo; medir desempenho; corrigir falhas | Todos |
| 10 | 11 – 15 out | Documentação e entrega | gerar diagramas de componentes e sequência; consolidar relatório técnico em PDF; preparar repositório GitHub com plano de iteração, issues e instruções de build | GP & Todos |

## Critérios de avaliação

- **Cobertura funcional**: todos os contextos previstos devem estar
  parcialmente implementados e conectados. As APIs devem permitir
  cadastro de usuários e licenças, cálculo de fundações, geração de
  parecer e sincronização offline.  
- Qualidade arquitetural: as implementações devem seguir o padrão
  Ports & Adapters, com separação de comandos/consultas e dependências
  invertidas; o motor de cálculo deve permanecer desacoplado.  
- Desempenho offline: o protótipo deve funcionar sem conexão,
  utilizando banco SQLite e produzindo eventos para sincronização futura.  
- Rastreabilidade: cada ação (cálculo, pagamento, sincronização)
  deve gerar eventos e logs para auditoria.  
- Documentação: diagramas e manual de uso devem estar completos,
  facilitando a evolução da próxima iteração.  

## Riscos e estratégias de mitigação

- Complexidade dos algoritmos de cálculo – as fórmulas geotécnicas
  podem ser extensas. Começar com casos simples (sapatas isoladas) e
  validar os resultados com exemplos de engenharia.  
  Integração de pagamentos – depender de gateways externos pode gerar
  atrasos; utilizar sandbox de pagamento e simular callbacks para
  desenvolver o módulo Negócio.  
- Conflitos de sincronização – divergências de dados entre o banco
  local e o servidor central podem causar perda de informação; adotar
  semântica de agregados, idempotência e checkpoints conforme descrito
  no contexto de Sincronização.  
  
## Referências

Este plano foi elaborado com base no Documento de Arquitetura de
Software – FundCalc, que descreve os objetivos e módulos da
aplicação】.  Para maior
contexto, consultar o diagrama de componentes e os módulos principais
listados no documento.

