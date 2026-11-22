# Pesquisa — Como verificar se a fábrica está pronta

## Formato e posição da seção

**Decisão.** Inserir uma seção de nível 2 entre “Rodar no projeto da aplicação” e “Acionar a fábrica”, com uma orientação para executar na raiz e uma tabela de comandos e resultados.

**Justificativa.** A posição segue a premissa aprovada. A tabela associa cada comando ao resultado esperado e ao sinal de pendência.

**Alternativas avaliadas.** Incorporar a lista à configuração trataria a verificação como parte da instalação. Um script de diagnóstico ampliaria a manutenção do repositório. A seção dedicada atende ao título solicitado e documenta a conferência no ponto de uso.

**Rastreabilidade.** US1, FR-001 e SC-001.

## Comandos e interpretação

**Decisão.** Documentar os quatro comandos aprovados como consultas independentes. Usar `main` como exemplo da branch base deste repositório e orientar a comparação com a branch da demanda ao retomar.

**Justificativa.** `git branch --show-current` informa a branch atual e pode retornar saída vazia com `HEAD` destacado. `git status --porcelain` distingue uma árvore limpa de uma árvore com mudanças, mesmo com código `0` nos dois casos. `git remote get-url origin` consulta a URL configurada. `gh auth status` permite conferir a autenticação no host esperado.

**Alternativas avaliadas.** Fixar `main` para todos os usos deixaria de cobrir a retomada. Consultar o remoto pela rede ou documentar o login atenderia a cenários além da conferência aprovada.

**Rastreabilidade.** US1, FR-002 a FR-005, SC-002 e SC-003.

## Estratégia de verificação

**Decisão.** Validar o título e os comandos no corpo da seção com PowerShell, conferir o diff com Git e usar o quickstart para verificar resultados positivos e divergentes. A consulta autenticada faz parte do aceite manual.

**Justificativa.** O teste estrutural depende apenas do documento. O aceite manual permite comparar cada comando com a saída e identificar pendências. Os exemplos de divergência exercitam a interpretação com a configuração da sessão preservada.

**Alternativas avaliadas.** Uma leitura do Markdown teria apenas verificação manual. Automatizar cenários por meio de alterações de branch, remoto ou autenticação exigiria preparar estados adicionais. A combinação de checagem estrutural e aceite usa as ferramentas existentes.

**Rastreabilidade.** FR-001 a FR-005 e SC-001, SC-002 e SC-003.
