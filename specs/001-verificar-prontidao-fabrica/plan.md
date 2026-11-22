# Plano de implementação — Como verificar se a fábrica está pronta

- **Branch** `001-verificar-prontidao-fabrica`
- **Data** 2026-11-22
- **Entrada** [Especificação aprovada](spec.md)

## Resumo

Adicionar ao `README.md` a seção “Como verificar se a fábrica está pronta”, entre “Rodar no projeto da aplicação” e “Acionar a fábrica”. Orientar a execução na raiz do repositório e apresentar cada comando copiável junto de seu resultado esperado e de um sinal de pendência. A seção atende a quem inicia uma demanda e a quem retoma uma feature. [Decisões](research.md)

## Contexto técnico

| Aspecto | Decisão |
| --- | --- |
| Linguagem e formato | Markdown do README, com comandos de terminal em código inline. |
| Dependências | Git e GitHub CLI. O aceite da autenticação usa o host GitHub do projeto. PowerShell executa o teste estrutural. |
| Plataforma | Os comandos usam o Git e o GitHub CLI existentes. O roteiro de validação usa PowerShell, conforme a configuração do repositório. |
| Escopo | Uma seção do README e quatro verificações. A consulta de `origin` confirma a configuração local. A de `gh` consulta o estado da autenticação. |
| Testes | Teste estrutural obrigatório e roteiro de aceite em `quickstart.md`, no diretório da feature. |

## Constitution Check

| Princípio | Antes da pesquisa | Após o desenho |
| --- | --- | --- |
| I. Integração da fábrica no Codex | Conforme. O objetivo é melhorar a documentação para acionar a fábrica. | Conforme. A seção segue o fluxo de prontidão e usa os comandos aprovados. |
| II. Especificação rastreável | Conforme. A aprovação de E1 está registrada na branch. | Conforme. O desenho e o aceite referenciam US1, FR-001 a FR-005 e SC-001, SC-002 e SC-003. |
| III. Delegação sequencial e estado retomável | Conforme. A sessão principal conferiu a branch e a aprovação antes de acionar o Arquiteto. | Conforme. A sessão principal valida E2 e registra o estado para retomada após a revisão solicitada. |
| IV. Verificação conforme a mudança | Conforme. O plano prevê verificações da documentação e de seu uso. | Conforme. O teste obrigatório verifica a estrutura, e o roteiro verifica a interpretação das saídas. |
| V. Credenciais com acesso mínimo | Conforme. A sessão usa a autenticação existente. | Conforme. O teste estrutural usa apenas o arquivo local. No aceite da autenticação, registre o resultado da verificação e o código de saída. |

## Estrutura do projeto

```text
README.md
specs/001-verificar-prontidao-fabrica/
├── plan.md
├── research.md
└── quickstart.md
```

A seção usa um título de nível 2, uma orientação breve para executar na raiz do repositório e uma tabela que associa cada comando à interpretação da saída.

| Requisito | Conteúdo a documentar | Aceite |
| --- | --- | --- |
| FR-001 | Título exato e quatro pares de comando e resultado esperado na seção. | Quickstart, etapas 1 e 2. SC-001. |
| FR-002 | `git branch --show-current` mostra a branch base ao iniciar ou a branch da feature ao retomar. Nome divergente ou saída vazia sinaliza pendência. | Quickstart, etapas 2 a 4. SC-001, SC-002 e SC-003. |
| FR-003 | `git status --porcelain` com saída vazia e execução bem-sucedida indica árvore limpa. Linhas na saída indicam alterações ou arquivos não rastreados. | Quickstart, etapas 2 a 4. SC-001, SC-002 e SC-003. |
| FR-004 | `git remote get-url origin` mostra a URL configurada para `origin`. Erro por ausência do remoto sinaliza pendência. | Quickstart, etapas 2 a 4. SC-001, SC-002 e SC-003. |
| FR-005 | `gh auth status` confirma conta autenticada no host esperado e código `0`. Falha de autenticação ou indisponibilidade do executável sinaliza pendência. | Quickstart, etapas 2 a 4. SC-001, SC-002 e SC-003. |

## Verificações

Após a implementação, execute o **teste obrigatório** no PowerShell, a partir da raiz do repositório. Este é o comando registrado em `state.json.commands.test`.

```powershell
& {
    $readme = Get-Content -LiteralPath 'README.md' -Raw -Encoding UTF8 -ErrorAction Stop
    $sections = [regex]::Matches($readme, '(?ms)^## Como verificar se a fábrica está pronta\r?\n(?<section>.*?)(?=^#{1,2}[ \t]+|\z)')
    if ($sections.Count -ne 1) {
        throw 'Esperava exatamente uma seção Como verificar se a fábrica está pronta.'
    }
    $section = $sections[0].Groups['section'].Value
    foreach ($command in @('git branch --show-current', 'git status --porcelain', 'git remote get-url origin', 'gh auth status')) {
        if (-not $section.Contains(('`' + $command + '`'))) {
            throw "Comando ausente na seção ($command)."
        }
    }
    Write-Output 'Título confirmado e 4 de 4 comandos encontrados.'
}
```

O teste confirma que o título aparece uma vez e que os quatro comandos estão escritos como código inline na seção. Execute também `git diff --check main -- README.md` e espere código `0`. A conferência detecta problemas de espaços em branco no diff contra a base da feature.

Use o [roteiro de aceite](quickstart.md) para conferir a posição da seção, a associação dos comandos aos resultados e a interpretação de resultados divergentes.

O escopo documental cobre o aceite manual e o teste estrutural. Teste unitário, lint configurado e build ou typecheck são inaplicáveis.

## Controle de complexidade

O desenho atende aos cinco princípios da constituição.
