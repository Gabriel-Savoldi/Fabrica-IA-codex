# Fábrica de Software Agêntica no Codex

Adaptação da fábrica de software da disciplina para o OpenAI Codex. A sessão principal conduz o fluxo de E0 a E8. O GitHub Spec Kit gera os artefatos e o Git organiza a entrega em uma branch por demanda.

## Configuração

| Arquivo ou pasta | Função |
| --- | --- |
| `AGENTS.md` | Orientação do projeto para encontrar e usar a fábrica. |
| `.codex/config.toml` | Ativa a delegação nativa com um subagente aberto por vez. A sessão usa escrita no workspace e aprovação de ferramentas sob demanda. |
| `.codex/agents/` | Seis papéis em TOML, com responsabilidades e escopo de trabalho definidos em `developer_instructions`. |
| `.agents/skills/fabrica-software/` | Skill que orquestra o fluxo, os checkpoints, as devoluções e o estado retomável. |
| `.agents/skills/speckit-*/` e `.specify/` | Skills, templates e scripts PowerShell do Spec Kit 1.4.3 para o Codex. |

Os subagentes herdam as permissões da sessão principal. Cada TOML delimita o que seu papel pode alterar e o orquestrador confere branch, commits e arquivos após cada acionamento. O modelo é selecionado na sessão do Codex.

## Rodar no projeto da aplicação

1. Coloque a configuração e as skills na raiz do projeto que receberá as demandas. Ao usar a fábrica em outro projeto, mescle `AGENTS.md`, `.codex/`, `.agents/` e `.specify/` com os arquivos já existentes. Se o outro projeto já tiver o Spec Kit inicializado, use as skills Codex desse projeto e preserve a constituição dele.
2. Abra essa raiz como projeto no Codex e confira a configuração de confiança quando a interface solicitar. No terminal, `codex` pode ser iniciado a partir da raiz.
3. Para iniciar o fluxo completo, use um repositório com remoto `origin` no GitHub e `gh` autenticado. Com o Codex aberto nessa raiz, defina os princípios do projeto com `$speckit-constitution`. O `constitution.md` desta adaptação é o modelo inicial do Spec Kit. Registre a configuração e a constituição em um commit da branch base antes da primeira demanda.

Se precisar inicializar o Spec Kit 1.4.3 em um novo projeto Windows, o comando verificado foi:

```powershell
specify init --here --script ps --integration codex '--integration-options=--skills' --non-interactive
```

## Acionar a fábrica

Para iniciar pelos requisitos, envie ao Codex uma mensagem como esta.

```text
$fabrica-software Especifique a exibição da data e hora do servidor em pt-BR, em vermelho, no fuso America/Sao_Paulo, sem atualização automática. Pare para minha revisão.
```

O Analista entrega a `spec.md` e a sessão principal aguarda o **Checkpoint 1**. Uma resposta como `Aprovo a especificação. Vou pedir a implementação depois.` registra a aprovação e deixa a feature pronta para retomar. Quando quiser continuar, indique a spec e até onde o Codex está autorizado a avançar.

```text
$fabrica-software Retome a spec aprovada 001-hora-servidor-pt-br e prossiga até a abertura e validação do pull request.
```

O orquestrador delega plano ao Arquiteto, tarefas ao Criador de Tarefas, código ao Desenvolvedor, revisão ao Revisor e testes de aceite ao QA. As etapas avançam conforme os arquivos e testes validados. No **Checkpoint 2**, a pessoa responsável faz o merge pelo GitHub. Uma nova sessão pode retomar o estado de uma feature pelo `specs/<feature>/state.json` na branch correspondente.

O identificador da segunda mensagem é ilustrativo. Use o identificador registrado na primeira etapa.

[Subagentes no Codex](https://learn.chatgpt.com/docs/agent-configuration/subagents) · [Skills no Codex](https://learn.chatgpt.com/docs/build-skills) · [GitHub Spec Kit](https://github.com/github/spec-kit)
