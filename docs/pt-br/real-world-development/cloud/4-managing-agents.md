---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O agente de nuvem funciona de forma assíncrona e autônoma, mas você ainda precisa acompanhar suas tarefas. A [página de agentes][agents-page] no GitHub reúne todas as tarefas com pull requests abertas. Você pode revisar as operações realizadas e até orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar tarefas do agente de nuvem;
- orientar uma sessão ativa para solicitar funcionalidade adicional.

## Cenário

Depois de solicitar o modo de alto contraste, a equipe decidiu adicionar também um modo claro. Como o trabalho já atualizava os estilos e criava controles, faz sentido incluir ambos. Você orientará o agente para adicionar o modo claro junto ao modo de alto contraste.

## Revise as tarefas do agente

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Examine as tarefas nos painéis principal e lateral. Você deverá ver:
   - Atualização da documentação.
   - Geração de APIs para modificar jogos.
   - Adição do modo de alto contraste.
3. Selecione uma tarefa em execução e revise as operações realizadas, como checkout do código, preparação do ambiente e configuração de servidores MCP.

> [!NOTE]
> As etapas exatas variam conforme o estado da execução e a abordagem adotada pelo agente.

## Oriente o agente de nuvem

1. Selecione a sessão que adiciona o modo de alto contraste.
2. Aguarde até que o agente indique que terminou a preparação e iniciou o trabalho.
3. No diálogo **Steer active session while Copilot is working**, envie:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

4. Selecione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o agente reconhecer o pedido e incorporar a nova solicitação ao fluxo.

## Resumo

Você acompanhou tarefas do agente de nuvem e redirecionou uma sessão ativa para ampliar o escopo.

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
