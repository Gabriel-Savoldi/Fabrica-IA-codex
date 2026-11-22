# Roteiro de aceite — Como verificar se a fábrica está pronta

Conduza este roteiro depois que a seção estiver no `README.md`. Abra o PowerShell na raiz do repositório com Git, GitHub CLI, remoto `origin` e autenticação configurados no host GitHub do projeto.

## 1. Conferir o documento

Execute o teste obrigatório do [plano](plan.md#verificações). Espere a mensagem `Título confirmado e 4 de 4 comandos encontrados.` e execução bem-sucedida. Execute também `git diff --check main -- README.md` e espere código `0`.

## 2. Conferir a apresentação

Abra `README.md` e localize o título exato “Como verificar se a fábrica está pronta” entre “Rodar no projeto da aplicação” e “Acionar a fábrica”. Confirme que o texto orienta a execução na raiz e que cada verificação associa um comando copiável à saída esperada e ao sinal de pendência. Esta etapa cobre US1 e SC-001.

## 3. Comparar os resultados positivos

Em uma sessão com a branch esperada e a árvore limpa, execute cada comando e compare com o README. Para uma demanda nova, use a branch base do projeto. Para uma retomada, use o nome registrado para a demanda.

| Comando | Resultado esperado |
| --- | --- |
| `git branch --show-current` | Nome da branch esperada. Neste repositório, `main` é a base para uma demanda nova. |
| `git status --porcelain` | Saída vazia e código `0`. |
| `git remote get-url origin` | URL configurada para `origin` e execução bem-sucedida. |
| `gh auth status` | Confirmação de conta autenticada no host GitHub do projeto e código `0`. |

Confira `$LASTEXITCODE` logo após o comando correspondente quando o código de saída for parte da verificação. Registre se as quatro saídas correspondem ao que a seção descreve. Esta etapa cobre FR-002 a FR-005 e SC-002.

## 4. Interpretar divergências

Leia a seção e identifique a pendência apontada em cada exemplo. Avalie os exemplos preservando a branch, o remoto e a autenticação da sessão.

| Exemplo | Pendência que a seção deve permitir identificar |
| --- | --- |
| O comando da branch retorna um nome diferente do esperado ou saída vazia com `HEAD` destacado. | Conferir a branch da operação. |
| O comando do estado exibe ` M README.md` com código `0`. | A árvore tem alteração local. Uma linha começando com `??` indica um arquivo não rastreado. |
| A consulta de `origin` retorna erro por ausência do remoto. | A configuração de `origin` está pendente. |
| `gh auth status` indica falha de autenticação, ou o terminal informa que `gh` está indisponível. | A autenticação do GitHub CLI está pendente. |

Registre o resultado das quatro interpretações para SC-003. Inclua apenas o estado da verificação e os códigos de saída pertinentes no relatório de aceite.
