---
title: "Lição 4 - Oriente o Copilot com instruções personalizadas"
description: "Crie e aplique instruções específicas do repositório para orientar o trabalho de filtragem no Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Nesta lição, você explorará instruções personalizadas e verá como elas influenciam as alterações geradas pelo Copilot.

## Revise as instruções existentes

1. Abra `.github/copilot-instructions.md`.
2. Revise os padrões globais do projeto.
3. Abra `.github/instructions` e examine os arquivos aplicáveis aos testes, à interface e à camada de dados.

## Adicione uma instrução ao repositório

Crie uma regra que reflita um padrão necessário da equipe. Por exemplo, defina que novas funcionalidades tenham testes, comentários TSDoc e cabeçalhos de arquivo conforme as convenções existentes.

Salve a alteração e confirme-a no branch compartilhado de filtragem.

## Aplique as instruções ao trabalho

Inicie uma nova conversa no Copilot CLI e peça ao agente que revise a filtragem existente à luz das instruções. Solicite correções apenas quando houver uma divergência real e execute as verificações relevantes.

> [!TIP]
> Mantenha instruções claras, específicas e acionáveis. Evite regras contraditórias entre arquivos com escopos diferentes.
