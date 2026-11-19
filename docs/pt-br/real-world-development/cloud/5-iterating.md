---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisão do trabalho

Neste workshop, você trabalhou com o GitHub Copilot em várias tarefas que melhoram a experiência do usuário e adicionam funcionalidades. Você solicitou documentação, o recurso de jogos relacionados e recursos de acessibilidade com modos de alto contraste e claro. Agora você revisará as alterações e fornecerá feedback quando necessário.

## Segurança do agente de nuvem

Como o agente de nuvem executa tarefas de forma assíncrona e sem supervisão direta, existem restrições de segurança:

- O agente tem acesso de leitura ao repositório e acesso de gravação somente ao branch criado para o trabalho.
- O agente executa em GitHub Actions em um ambiente separado e temporário.
- Workflows do GitHub Actions exigem aprovação humana.
- O [acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive servidores MCP.

## Revise a documentação gerada

1. Retorne ao repositório no github.com.
2. Selecione **Pull Requests**.
3. Abra a pull request criada para adicionar documentação.
4. Se ela tiver o indicador **[WIP]**, aguarde a conclusão do trabalho.
5. Selecione a guia **Files changed** e revise as alterações.
6. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de arquivo.
7. Quando a revisão estiver pronta, volte à guia **Conversation** e aprove os workflows necessários, se solicitado.

## Solicite alterações

Você pode solicitar alterações ao Copilot por comentários na pull request ou em linhas específicas do código.

- Adicione cabeçalhos de comentário no início dos arquivos.
- Adicione comentários TSDoc aos arquivos TypeScript e Astro.
- Crie um README que descreva a estrutura do aplicativo Astro.

Selecione **View Session** para acompanhar uma nova sessão criada pelo Copilot e depois volte à pull request para revisar o novo commit.

## Revise os jogos relacionados e os recursos de acessibilidade

Revise separadamente as pull requests relacionadas aos jogos relacionados e aos modos de alto contraste e claro. Execute as verificações necessárias, teste o comportamento no codespace ou no navegador e solicite ajustes por comentários quando necessário. Depois da revisão, marque o rascunho como pronto e mescle as alterações aprovadas.

## Resumo

Você revisou o trabalho assíncrono do Copilot, aprovou verificações, solicitou alterações e iterou sobre pull requests até que o resultado estivesse pronto para fusão.

[previous-lesson]: ../4-managing-agents/
