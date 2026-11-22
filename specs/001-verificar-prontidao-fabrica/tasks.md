---
description: "Tarefas de implementação da seção de prontidão da fábrica"
---

# Tarefas — Como verificar se a fábrica está pronta

**Insumos** [Especificação](spec.md), [plano técnico](plan.md), [pesquisa](research.md) e [roteiro de aceite](quickstart.md).

## Fase 1 — Preparação

A implementação usa o `README.md` existente e as ferramentas descritas em `plan.md`. A preparação do projeto já está atendida.

## Fase 2 — Fundação

O plano aprovado fornece o formato, a posição da seção e o comando de verificação. US1 pode começar com esses insumos.

## Fase 3 — US1 · Verificar a prontidão antes de acionar a fábrica (P1, MVP)

**Objetivo.** Permitir que a pessoa confira branch, árvore de trabalho, `origin` e autenticação do GitHub CLI pelo README.

**Teste independente.** A partir da raiz do repositório, execute o teste estrutural de `plan.md` para confirmar o título e os quatro comandos dentro da seção. O roteiro de `quickstart.md` complementa a checagem ao conferir o resultado esperado junto de cada comando e a interpretação de resultados divergentes.

- [ ] T001 [US1] Adicionar em `README.md` o título de nível 2 “Como verificar se a fábrica está pronta”, entre “Rodar no projeto da aplicação” e “Acionar a fábrica”, e orientar a execução na raiz do repositório (FR-001, SC-001).
- [ ] T002 [US1] Preencher a seção de `README.md` com uma tabela dos quatro comandos copiáveis e seus resultados esperados e sinais de pendência. Cobrir branch base ou da feature e saída vazia em `HEAD` destacado, árvore limpa com saída vazia e código `0` ou linhas de alteração, URL configurada de `origin` ou erro por ausência, e conta autenticada no host GitHub esperado com código `0` ou falha de autenticação ou de disponibilidade do `gh` (FR-001 a FR-005, SC-001 a SC-003).
- [ ] T003 [US1] Verificar `README.md` em E4 executando o teste estrutural de `plan.md`, registrado em `state.json` como `commands.test`, e `git diff --check main -- README.md`. Confirmar título único, quatro comandos na seção e código `0` no diff (FR-001, SC-001).

## Fase 4 — Acabamento e aceite

A verificação documental de E4 está incluída em T003. Em E6, o QA aplica `quickstart.md` para conferir a apresentação, comparar as quatro saídas positivas e interpretar os quatro exemplos divergentes (US1, FR-001 a FR-005, SC-001 a SC-003).

## Dependências e ordem de execução

- A sequência é T001 → T002 → T003. Os resultados de T003 encerram a verificação de implementação de US1.
- O aceite de E6 ocorre depois de E4. US1 é a única história e pode ser validada como um incremento completo.
- As edições recaem sobre a mesma seção de `README.md`, e a verificação depende dessas edições. A execução é sequencial.

## Estratégia de implementação

O MVP é a entrega integral de US1. A seção reúne os quatro comandos e suas interpretações no ponto de uso, com o teste estrutural e a conferência do diff executados antes de encaminhar o trabalho ao aceite.
