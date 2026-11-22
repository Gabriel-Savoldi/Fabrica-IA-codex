# Especificação da feature — Como verificar se a fábrica está pronta

- **Branch da feature** `001-verificar-prontidao-fabrica`
- **Criada em** 2026-11-22
- **Status** Rascunho
- **Entrada** Especificar uma seção no README com os comandos para conferir a branch atual, a árvore de trabalho, o remoto `origin` e a autenticação do `gh`, com o resultado esperado de cada comando.

## Cenários de uso e testes

### US1 — Verificar a prontidão antes de acionar a fábrica (Prioridade P1)

Como pessoa que vai iniciar ou retomar uma demanda, quero consultar o README para verificar em qual branch estou, se a árvore de trabalho está limpa, se `origin` está configurado e se o GitHub CLI está autenticado.

As quatro verificações permitem identificar pendências da sessão antes de acionar a fábrica.

**Teste independente.** Localizar a seção pelo título exato, executar as quatro verificações a partir da raiz de um repositório com a fábrica configurada e comparar cada saída com o resultado esperado no README.

**Cenários de aceitação**

1. **Dado** um repositório pronto para iniciar uma demanda, **quando** a pessoa executa as verificações da seção, **então** ela identifica a branch base, uma saída vazia para a árvore de trabalho, o endereço configurado de `origin` e uma conta autenticada no GitHub com código de saída `0`.
2. **Dado** que uma verificação apresenta um resultado diferente do esperado, **quando** a pessoa compara a saída com o README, **então** ela identifica qual das quatro verificações está pendente.

### Casos de borda

- Em `HEAD` destacado, a verificação da branch pode retornar uma saída vazia. A pessoa deve distinguir esse resultado do nome da branch esperada.
- Uma linha na saída da verificação da árvore indica alteração ou arquivo não rastreado, mesmo quando o comando termina com código `0`.
- Se `origin` estiver ausente, o comando apresenta erro. Se `gh` estiver indisponível ou o estado de autenticação indicar falha, a autenticação segue pendente.

## Requisitos

- **FR-001** — O README deve conter uma seção intitulada exatamente “Como verificar se a fábrica está pronta”, com um comando copiável e um resultado esperado junto de cada uma das quatro verificações.
- **FR-002** — A seção deve apresentar `git branch --show-current` e indicar como resultado esperado o nome da branch pretendida para a operação. Para uma demanda nova, ela é a branch base escolhida, como `main` neste repositório. Para retomar uma demanda, ela é a branch da feature registrada para aquela demanda.
- **FR-003** — A seção deve apresentar `git status --porcelain` e indicar que saída vazia com execução bem-sucedida significa árvore de trabalho limpa. Linhas na saída indicam alterações ou arquivos não rastreados.
- **FR-004** — A seção deve apresentar `git remote get-url origin` e indicar como resultado esperado a URL configurada para o repositório no remoto `origin`.
- **FR-005** — A seção deve apresentar `gh auth status` e indicar como resultado esperado a confirmação de uma conta autenticada no host GitHub usado pelo projeto, com código de saída `0`.

## Critérios de sucesso

- **SC-001** — A revisão do README encontra a seção pelo título solicitado e localiza um comando com seu resultado esperado em 4 de 4 verificações.
- **SC-002** — Em um repositório pronto para uma demanda nova, a comparação das saídas documentadas com os resultados reais confirma 4 de 4 verificações.
- **SC-003** — Ao comparar quatro exemplos de resultados divergentes, um por verificação, a pessoa identifica em todos os exemplos qual item está pendente.

## Premissas

- A seção fica depois de “Rodar no projeto da aplicação” e antes de “Acionar a fábrica”, onde serve como conferência antes do acionamento.
- O público executa os comandos a partir da raiz do repositório e já passou pela configuração inicial da fábrica.
- Os quatro comandos são consultas. A verificação do remoto confirma sua configuração local, e a verificação de autenticação confirma o estado da conta no host.
