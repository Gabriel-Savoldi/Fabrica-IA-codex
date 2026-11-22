# Fabrica-IA-codex Constitution

## Core Principles

### I. Integração da fábrica no Codex

As mudanças neste repositório devem atender à adaptação da fábrica para o Codex e aos critérios
da demanda aprovada. A integração usa a configuração e os subagentes locais do Codex, as skills
do Spec Kit e o Git para organizar a entrega.

### II. Especificação rastreável

Cada demanda da fábrica deve ter uma especificação com critérios de sucesso verificáveis. O
plano, as tarefas e os testes devem referenciar os requisitos e as histórias que cobrem. Use os
artefatos do Spec Kit para registrar requisitos, decisões e esclarecimentos.

### III. Delegação sequencial e estado retomável

A sessão principal deve acionar um subagente por vez, com um escopo definido. Antes de avançar,
o orquestrador deve conferir os artefatos e verificações da etapa. A branch da demanda e o
`state.json` registram o trabalho e o ponto de retomada.

### IV. Verificação conforme a mudança

O plano deve declarar um comando de teste que verifique o comportamento alterado. Mudanças na
fábrica devem ser conferidas no formato correspondente e no fluxo que afetam. O orquestrador
deve registrar os comandos executados e seus resultados antes de apresentar a entrega.

### V. Credenciais com acesso mínimo

As operações autenticadas devem usar credenciais com as permissões necessárias para o
repositório da demanda. Mantenha tokens e chaves em mecanismos de credenciais da sessão,
fora dos arquivos versionados e dos relatórios.

## Escopo do repositório

Este repositório contém a adaptação da fábrica para o Codex. O código e as regras de negócio
de uma aplicação de demonstração pertencem ao projeto em que a aplicação for testada.

## Fluxo de trabalho

O Analista entrega a especificação para aprovação humana. Após a aprovação e o pedido para
prosseguir, a fábrica executa plano, tarefas, análise, implementação, revisão e testes de
aceite. Com autorização para publicar, o orquestrador abre o PR e verifica a CI quando
configurada. O merge na branch base cabe à pessoa responsável.

## Governance

O plano deve verificar a conformidade com estes princípios e justificar eventuais exceções.
A pessoa responsável revisa as alterações da constituição no PR com sua motivação e versão.
Use versão maior para mudanças incompatíveis, versão menor para novos princípios e versão de
correção para ajustes de redação.

**Version**: 1.0.0 | **Ratified**: 2026-11-22 | **Last Amended**: 2026-11-22
