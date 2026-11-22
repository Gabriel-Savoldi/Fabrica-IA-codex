# Fábrica de Software Agêntica

Use a skill `$fabrica-software` quando o usuário pedir para iniciar, esclarecer ou retomar uma demanda pela fábrica. Nesse fluxo, a sessão principal do Codex orquestra os seis subagentes definidos em `.codex/agents/`.

Cada subagente executa a etapa indicada pelo orquestrador e devolve o resultado à sessão principal. Pedidos de leitura, configuração ou manutenção da fábrica seguem o escopo do pedido.
