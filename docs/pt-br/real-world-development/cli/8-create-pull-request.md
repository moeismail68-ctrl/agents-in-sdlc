---
title: "Lição 8 - Criar e mesclar a pull request do recurso"
description: "Revise a alteração completa de filtragem, crie uma pull request e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Nesta lição, você reunirá a alteração completa da filtragem em uma pull request e concluirá a revisão.

## Revise a alteração

Antes da fusão, confirme:

- os requisitos da issue foram atendidos;
- os testes cobrem os cenários principais;
- as instruções personalizadas foram aplicadas;
- o Playwright MCP forneceu evidências úteis;
- o agente de QA não encontrou problemas pendentes.

Execute `/diff` novamente e revise todos os arquivos alterados.

## Crie a pull request

1. Confirme a alteração no branch compartilhado.
2. Execute `/pr create`.
3. Revise o título, a descrição e as verificações da PR.
4. Use o Agent Merge somente depois que os requisitos e as verificações forem aprovados.
5. Confirme a fusão no GitHub.
