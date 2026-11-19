---
title: "Lição 7 - Criar e usar um agente de QA"
description: "Crie um agente especializado para revisar requisitos, cobertura de testes e evidências de verificação da filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Nesta lição, você criará um agente personalizado de garantia de qualidade (QA) e o usará para revisar a alteração de filtragem.

## Crie o perfil do agente

1. Crie `.github/agents/qa.md` no repositório.
2. Defina campos obrigatórios `name` e `description`.
3. Descreva as responsabilidades do agente: revisar requisitos, identificar lacunas de cobertura e verificar evidências.

O agente de QA deve ser crítico, mas não alterar o escopo da funcionalidade por conta própria.

## Revise o trabalho

Selecione o agente de QA no Copilot CLI e peça que ele:

- confirme que a implementação cobre todos os requisitos da issue;
- identifique cenários sem testes;
- revisa as verificações executadas;
- liste quaisquer problemas restantes por prioridade.

> [!CAUTION]
> O agente de QA pode encontrar problemas ou omissões; você continua responsável pela decisão final e pela revisão das alterações.
