# Parecer de teste e aceite — E6

- **Feature e branch** `001-verificar-prontidao-fabrica`
- **Commit testado** `5a0ab966f8abb5c6abe39b7d62a2bb46fb959907`
- **Commit aprovado em E5** `4227bb2303d93eed8e7c8687f8b65b168858cdde`
- **Parecer de QA** aprovado

## Verificações executadas

| Etapa | Comando | Código | Resultado observado |
| --- | --- | --- | --- |
| 1 | Teste PowerShell de `state.json.commands.test` | `0` | Título confirmado e 4 de 4 comandos encontrados. |
| 1 | `git diff --check main -- README.md` | `0` | Diferenças do README verificadas sem erros. |
| 3 | `git branch --show-current` | `0` | `001-verificar-prontidao-fabrica`, conforme o cenário de retomada. |
| 3 | `git status --porcelain` | `0` | Saída vazia, conforme a condição de árvore limpa. |
| 3 | `git remote get-url origin` | `0` | Retornou a URL configurada para `origin`. |
| 3 | `gh auth status` | `0` | Conta ativa autenticada no host esperado. |

Na etapa 2, a inspeção do README confirmou o título exato “Como verificar se a fábrica está pronta” entre “Rodar no projeto da aplicação” e “Acionar a fábrica”. A seção orienta a execução na raiz e associa cada comando copiável ao resultado esperado e ao sinal de pendência.

Para o cenário de demanda nova, a sessão principal executou os quatro comandos na `main` antes do acionamento do QA. Todos retornaram código `0`. Os resultados foram o nome `main`, a árvore limpa, `origin` configurado e conta ativa autenticada no host esperado.

## Interpretação das divergências

Na etapa 4, o QA usou os exemplos de [quickstart.md](quickstart.md) e preservou a branch, o remoto e a autenticação da sessão.

| Exemplo do roteiro | Interpretação conferida no README | Resultado |
| --- | --- | --- |
| Branch diferente da esperada ou saída vazia em `HEAD` destacado | Conferir a branch da operação. | Aprovado |
| ` M README.md` com código `0`, ou linha que começa com `??` | Há alteração local ou arquivo não rastreado. | Aprovado |
| A consulta de `origin` retorna erro por ausência do remoto | A configuração de `origin` está pendente. | Aprovado |
| Falha de autenticação ou indisponibilidade de `gh` | A autenticação do GitHub CLI está pendente. | Aprovado |

## Cobertura e totais

| Critério | Requisitos | Evidência |
| --- | --- | --- |
| US1, SC-001 | FR-001 a FR-005 | Etapas 1 e 2 aprovadas. Título, posição, execução na raiz e quatro pares de comando e resultado conferidos. |
| US1, SC-002 | FR-002 a FR-005 | Etapa 3 aprovada. Quatro resultados de retomada conferidos pelo QA e quatro de demanda nova conferidos pela sessão principal. |
| US1, SC-003 | FR-002 a FR-005 | Etapa 4 aprovada. Quatro pendências identificadas pelos exemplos. |

- **Etapas do roteiro** 4 de 4 aprovadas.
- **Comandos executados pelo QA** 6 de 6 com código `0`.
- **Exemplos de divergência** 4 de 4 interpretados corretamente.
- **Arquivos de teste criados ou alterados** 0. O aceite usa o teste estrutural e o roteiro definidos no plano.
- **Defeitos de aceite** 0.
