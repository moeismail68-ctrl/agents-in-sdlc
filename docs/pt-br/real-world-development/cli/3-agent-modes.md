---
title: "Lição 3 - Modos do agente: Plan e Autopilot"
description: "Use os modos Plan, Autopilot e Interactive do Copilot CLI para planejar e implementar uma funcionalidade de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Nesta lição, você usará diferentes modos do Copilot CLI para planejar, implementar e revisar a funcionalidade de filtragem a partir de uma issue.

## Cenário

A equipe precisa filtrar os jogos por categoria na página de listagem. Comece pela issue do repositório, confirme os requisitos e defina uma abordagem antes de gerar código.

## Planeje a alteração

1. Abra uma nova conversa no Copilot CLI.
2. Selecione o modo **Plan**.
3. Peça ao agente que leia a issue de filtragem e prepare um plano sem fazer alterações.
4. Revise as decisões sobre arquivos, dados, interface e testes.

Use prompts claros para validar qualquer suposição antes de prosseguir.

## Implemente no modo Autopilot

Alterne para o modo **Autopilot** e peça ao agente que implemente o plano. O modo Autopilot permite executar as etapas necessárias sem solicitar confirmação a cada ferramenta.

Revise o diff com `/diff` e confirme:

- os filtros correspondem às categorias disponíveis;
- o estado vazio é tratado corretamente;
- os testes cobrem os principais cenários;
- as verificações do projeto passam.

> [!NOTE]
> Revise as alterações mesmo quando o agente estiver funcionando em modo autônomo. A aprovação automatizada não substitui uma revisão humana.

## Revise no modo Interactive

Alterne para o modo **Interactive**, solicite correções para quaisquer problemas encontrados e revise a solução final antes de criar a pull request.
