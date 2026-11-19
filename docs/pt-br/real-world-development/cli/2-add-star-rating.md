---
title: "Lição 2 - Adicionar avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards dos jogos, revise-a em um navegador encaminhado e mescle-a como sua primeira pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e tentou uma conversa, é hora de fazer sua primeira alteração no projeto. Você manterá a alteração pequena: os jogos já têm uma avaliação por estrelas nos dados, mas os cards da página inicial ainda não mostram essa informação. Você pedirá ao agente que a exiba, revisará a alteração e a mesclará como sua primeira pull request.

Nesta lição, você vai:

- iniciar uma conversa focada no Copilot em um branch de recurso;
- pedir ao agente que faça uma alteração pequena no projeto;
- revisar a alteração com `/diff`;
- executar o aplicativo para confirmar a alteração em um navegador encaminhado;
- abrir e mesclar sua primeira pull request.

## Cenário

Cada jogo no Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cards da página inicial mostram apenas título, categoria, editora e descrição. Como aquecimento, peça ao agente que exiba a avaliação existente em cada card — uma alteração pequena e autocontida, ideal para a primeira sessão.

## Inicie uma conversa

1. Retorne ao Codespace.
2. Inicie o Copilot com:

   ```bash
   copilot --yolo
   ```

3. Inicie uma nova sessão com `/new` e pressione <kbd>Enter</kbd>.
4. Use este prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

## Revise o diff

Use `/diff` e revise todos os arquivos alterados. Confirme que o card mostra a avaliação numérica quando ela existe e `No rating yet` quando `starRating` é `null`. Confirme também que os testes cobrem ambos os estados.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico. O código exato pode variar; revise o comportamento esperado em vez de procurar uma implementação específica.

## Verifique as alterações

Peça ao Copilot que inicie o aplicativo e use a URL encaminhada pelo Codespaces para confirmar que os cards mostram avaliações de cinco pontos.

## Abra e mescle a primeira pull request

1. Peça ao agente padrão que confirme a alteração.
2. Execute `/pr create`.
3. Abra a PR, revise os arquivos e as verificações.
4. Selecione **Merge pull request** e confirme a fusão.
5. Retorne ao Codespace e saia do Copilot CLI com `/exit`.

## Resumo

Você fez uma alteração pequena, revisou o diff, verificou o comportamento no navegador e mesclou sua primeira pull request.
