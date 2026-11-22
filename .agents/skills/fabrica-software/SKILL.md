---
name: fabrica-software
description: Orquestre uma demanda pela Fábrica de Software Agêntica no Codex. Use quando o usuário pedir para especificar, esclarecer, implementar ou retomar uma feature pelo fluxo da fábrica até o pull request.
---

# Orquestrar a Fábrica de Software

A sessão principal do Codex é o orquestrador. Ela delega cada etapa, valida os resultados no repositório e registra o andamento de uma feature em `specs/<feature>/state.json`. O fluxo tem dois checkpoints humanos, a aprovação da especificação e o merge do pull request.

## Começar ou retomar

1. Identifique qual feature e qual etapa o usuário pediu. Para E0, leia [Operação no repositório](references/repositorio.md#e0--pré-requisitos-e-branch-da-feature). Confirme o repositório Git com commit na base, `origin`, `gh` autenticado e a constituição preenchida. Em trabalho novo, confirme que a árvore de trabalho está limpa.
2. Para uma feature existente, localize `state.json` na branch da feature, leia os gates e retome da primeira etapa pendente. Quando `specs/<feature>/` estiver ausente na branch atual, localize a branch pelo identificador. Em `AGUARDANDO_REVISAO_HUMANA` ou `ESPECIFICACAO_APROVADA`, trate primeiro a mensagem humana. Em `AGUARDANDO_MERGE`, consulte o PR conforme E8. Em `BLOCKED`, use o motivo registrado para verificar se já pode retomar. Trate `ENTREGUE` e `CANCELADA` como estados finais.
3. Use as skills instaladas em `.agents/skills/speckit-*/SKILL.md` para os respectivos artefatos. Leia a skill da etapa e siga sua versão instalada. Se faltar uma skill, um subagente ou um pré-requisito, registre o bloqueio concreto e o que falta. Enquanto não existir diretório da feature, comunique o bloqueio na conversa. Preserve os artefatos gerados e mantenha as anotações de acompanhamento em `specs/<feature>/notes/`.

## Acionar um subagente

- Use a delegação nativa do Codex e selecione o nome exato da tabela. Mantenha apenas um subagente em atividade. Aguarde o retorno final, valide-o e encerre a sessão desse subagente antes de acionar o próximo. A sessão principal é responsável pela delegação, pelas trocas de branch e pelos commits.
- Antes de cada acionamento, mude para a branch da feature e confirme com `git branch --show-current`. Em E1 de uma feature nova, use a branch base. Para uma spec existente, confira também `.specify/feature.json`. Se apontar para outra spec, informe ao subagente o `SPECIFY_FEATURE_DIRECTORY` correto para executar os comandos da skill com essa variável no mesmo processo.
- Envie um handoff com o identificador, o diretório e a branch da feature, a etapa e a skill, os caminhos dos artefatos de entrada, as decisões aprovadas, as pendências para registrar, o escopo de edição, o diretório de notas e os comandos de verificação. Dê o escopo exato da rodada, como a fase de `tasks.md`, os apontamentos a corrigir ou o intervalo de commits a revisar. Oriente o subagente a retornar à sessão principal e trabalhar apenas nessa etapa.
- Compare o retorno com os arquivos no disco e confira branch, `HEAD` e arquivos alterados, inclusive os novos, antes de validar a etapa. Execute na sessão principal os comandos aplicáveis registrados em `state.json.commands` quando a etapa os exigir. Considere sucesso quando todos tiverem código de saída `0`.

Use este formato para o retorno ao orquestrador.

```text
STATUS: DONE | BLOCKED | NEEDS_CLARIFICATION | HUMAN_REVIEW_REQUIRED
PARECER: aprovado | aprovado com ressalvas | reprovado (somente Revisor e QA)
ARTEFATOS: caminhos criados ou alterados, um por linha
TESTES: comando executado e código de saída, ou não aplicável
BLOQUEIOS: itens, ou nenhum
PRÓXIMO: etapa sugerida, ou nenhum
```

## Etapas E1 a E8

| Etapa | Responsável | Evidência para avançar |
| --- | --- | --- |
| E1 · Especificação | `fabrica_analista_requisitos`, com `speckit-specify` ou `speckit-clarify` | `spec.md`, checklist de requisitos completo, dúvidas de requisito resolvidas e aprovação humana registrada. |
| E2 · Plano técnico | `fabrica_arquiteto`, com `speckit-plan` | `plan.md`, `research.md`, `quickstart.md` e artefatos aplicáveis. `Constitution Check` validado, comandos de verificação declarados e nenhuma lacuna bloqueante. |
| E3 · Tarefas | `fabrica_criador_tarefas`, com `speckit-tasks` | `tasks.md` no formato do Spec Kit, com tarefas ligadas às histórias e aos requisitos. |
| E3.5 · Consistência | Sessão principal, com `speckit-analyze` | Análise em leitura, cobertura dos requisitos funcionais e nenhum achado crítico pendente. |
| E4 · Implementação | `fabrica_desenvolvedor`, com `speckit-implement` | Uma fase de `tasks.md` por chamada ou uma rodada de correção. Todas as tarefas da fase marcadas e verificações aprovadas na sessão principal antes do commit. |
| E5 · Revisão | `fabrica_revisor_codigo` | `review-report.md` com parecer aprovado ou aprovado com ressalvas. |
| E6 · Teste e aceite | `fabrica_qa` | `qa-report.md` com comandos realmente executados e nenhum defeito bloqueante. O Revisor confere testes novos ou alterados pelo QA antes de E7. |
| E7 · PR e CI | Sessão principal | Branch sincronizada, PR aberto para a base correta e checks aprovados. Quando a CI estiver ausente, verificações locais aprovadas e ausência registrada. |
| E8 · Merge | Pessoa responsável | Confirmação do merge pelo GitHub e registro de `ENTREGUE`. |

### Checkpoint 1 · Aprovação da especificação

- Em E1, assim que a skill devolver a spec, crie ou confirme sua branch conforme a referência de [operação no repositório](references/repositorio.md). Registre `AGUARDANDO_REVISAO_HUMANA`, marque `human_checkpoints.specification_review` como pendente e faça commit da spec, do checklist e do estado. Se houver `NEEDS_CLARIFICATION`, encaminhe as perguntas ao humano e retome o Analista na mesma branch com as respostas.
- Quando a spec estiver pronta para decisão, apresente o caminho, um resumo dos requisitos e critérios de sucesso e as premissas. Peça a revisão da pessoa e encerre o turno. A evidência para E2 é o trecho literal de uma mensagem posterior do humano aprovando essa versão, salvo em `human_checkpoints.specification_review.mensagem`, acompanhado pela instrução para prosseguir.
- Se a pessoa aprovar sem pedir para prosseguir, grave `ESPECIFICACAO_APROVADA`, faça commit do registro e encerre. Uma solicitação posterior para continuar autoriza E2 com a aprovação registrada. Ao acionar o Arquiteto, cite a mensagem de aprovação no handoff. Uma permissão de ferramenta do Codex autoriza o comando correspondente. A aprovação da spec é registrada no checkpoint.
- Para revisar uma spec existente, volte ao Analista com `speckit-clarify` e passe o pedido do humano. Confira `## Clarifications`, atualize o checklist e reabra o checkpoint. Caso já haja plano, tarefas, código ou PR, registre que podem estar desatualizados. Após a nova aprovação, replaneje na mesma branch e atualize o PR existente em E7.

### Validação, correções e entrega

- Ao validar E2, registre o comando de teste obrigatório e lint e build/typecheck quando disponíveis em `state.json.commands`. Após E3.5, faça commit do plano e das tarefas. Devolva achados críticos ao responsável pelo artefato. Mudança de requisito reabre o Checkpoint 1.
- Em E4, valide e faça commit de cada fase concluída, registrando seus IDs em `tasks_done`. A primeira revisão abrange `<base_branch>...HEAD`. Nas rodadas seguintes, use `<last_reviewed_commit>..HEAD`. Registre o SHA efetivamente revisado quando E5 for aprovado. Se o QA criar ou alterar testes, passe os arquivos e suas alterações ao Revisor. Achado crítico nesses testes volta ao QA.
- Reprovações de implementação pelo Revisor, QA ou CI voltam ao Desenvolvedor na mesma branch e repetem E4, E5 e E6 com escopo de correção. Se um teste do QA for contestado, devolva a ele para corrigir o teste ou fundamentar o defeito. Um checklist de requisitos incompleto volta ao Analista. Checklists adicionais incompletos seguem a decisão do orquestrador e entram como ressalvas no PR. Pareceres aprovados com ressalvas permitem avanço.
- Dúvidas técnicas ficam com o subagente responsável e são registradas como premissas. Lacunas de requisito ou escopo voltam a E1. Limite a duas devoluções por etapa, registradas em `gates.<etapa>.devolucoes`. Limite a cinco rodadas pós-implementação que retornem ao Desenvolvedor por revisão, QA, CI ou sincronização. Rodadas técnicas pedidas pelo humano no PR seguem o Checkpoint 2. Quando faltar acesso, ferramenta ou decisão humana, ou o limite se esgotar, registre `BLOCKED` com a causa e a etapa a retomar.
- Após E6, registre `PUBLICANDO_PR` e siga [publicação e merge](references/repositorio.md#e7-e-e8--publicação-e-merge). Com as verificações de E7 aprovadas, apresente o PR, os testes executados, as ressalvas e a evidência dos gates. Registre `AGUARDANDO_MERGE` e encerre no Checkpoint 2. Um pedido técnico no PR reabre E4 a E7. Um pedido de requisito reabre E1. Ao confirmar o merge, registre `ENTREGUE`.

## Estado retomável

Use `state.json` na branch da feature. Atualize-o a cada avanço, fase, pausa e devolução. Preserve os registros existentes ao retomar. Use `null` até haver valor nos campos opcionais.

```json
{
  "feature": "<###-nome-curto>",
  "base_branch": "<branch de origem>",
  "branch": "<branch da feature>",
  "gh_path": "gh",
  "pull_request": null,
  "commands": {"test": null, "lint": null, "build": null},
  "last_reviewed_commit": null,
  "stage": "NOVA",
  "artifacts": {},
  "gates": {},
  "tasks_done": [],
  "blockers": [],
  "human_checkpoints": {},
  "updated_at": "<ISO-8601>"
}
```

Use os estados `NOVA`, `ESPECIFICANDO`, `AGUARDANDO_REVISAO_HUMANA`, `ESPECIFICACAO_APROVADA`, `PLANEJANDO`, `CRIANDO_TAREFAS`, `ANALISANDO`, `IMPLEMENTANDO`, `REVISANDO`, `TESTANDO`, `PUBLICANDO_PR`, `AGUARDANDO_MERGE`, `ENTREGUE`, `BLOCKED` e `CANCELADA`. Em `artifacts`, registre caminhos e etapas. Em `gates`, registre resultados das validações, devoluções, rodadas e premissas. Ao aprovar um checkpoint, salve `status`, `aprovado_em` e a mensagem literal da aprovação ou a confirmação de merge do GitHub. Para checkpoint pendente, use `{"status": "pendente"}`.

Em cancelamento solicitado pelo humano, registre o motivo e faça commit do estado na branch da feature. Deixe a branch e o PR disponíveis para decisão da pessoa responsável. Se precisar interromper a sessão, registre etapa, fase e ponto de retomada antes de responder.
