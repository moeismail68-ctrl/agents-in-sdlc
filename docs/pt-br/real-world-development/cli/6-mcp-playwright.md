---
title: "Lição 6 - Validar a funcionalidade com Playwright MCP"
description: "Configure o servidor MCP do Playwright no Copilot CLI e use-o para observar a filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Nesta lição, você adicionará o servidor Model Context Protocol (MCP) do Playwright ao Copilot CLI e o usará para observar o comportamento da interface.

## Configure o Playwright MCP

Siga as instruções do projeto para registrar o servidor MCP no ambiente da CLI. Reinicie o Copilot CLI e confirme que a ferramenta do Playwright está disponível.

## Observe a funcionalidade

1. Inicie o aplicativo.
2. Peça ao Copilot que abra a página de listagem no navegador.
3. Solicite a seleção de uma categoria e observe quais jogos permanecem visíveis.
4. Teste uma categoria sem resultados.
5. Capture evidências das etapas e dos resultados.

O agente pode usar o MCP para navegar, selecionar controles, registrar o estado da página e relatar o que encontrou.

> [!NOTE]
> Revisar o navegador é uma evidência adicional, não substitui testes automatizados e revisão manual do diff.
