---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de iniciar os exercícios do agente de nuvem do Copilot, prepare o ambiente. Você criará sua própria cópia do repositório Tailspin Toys e iniciará um [codespace][codespaces] para editar arquivos de instruções e revisar o trabalho produzido pelo agente de nuvem.

## Configure o repositório do laboratório

Para criar uma cópia do repositório no qual trabalhará, crie uma instância a partir do [modelo][template-repository]. A nova instância conterá todos os arquivos necessários para o laboratório e será usada durante os exercícios.

1. Em uma nova janela do navegador, acesse o [modelo Tailspin Toys][tailspin-template].
2. Crie sua própria cópia do repositório selecionando o botão **Use this template** na página do repositório do laboratório. Depois, selecione **Create a new repository**.

   ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Caso participe de um workshop conduzido pelo GitHub ou pela Microsoft, siga as instruções dos facilitadores. Caso contrário, crie o novo repositório em uma organização onde você tenha acesso ao GitHub Copilot.

   ![Configurações do repositório criado a partir do modelo](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório criado (**nome-da-organizacao-ou-usuario/nome-do-repositorio**), pois você precisará dele nas próximas lições.

## Crie um codespace

Em seguida, use um codespace para concluir os exercícios.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento baseado em nuvem que permite escrever, executar e depurar código diretamente no navegador.

1. Acesse o repositório recém-criado.
2. Selecione o botão verde **Code**.

   ![Botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e o botão **+** para criar um novo Codespace.

   ![Criação de um codespace](../../../_images/ex0-create-codespace.png)

> [!CAUTION]
> Você retornará ao codespace em um exercício futuro. Por enquanto, mantenha-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi preparado para ser executado em um codespace ou em um [dev container][dev-containers] local. Ambos garantem que o ambiente tenha os pré-requisitos necessários.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Resumo

Parabéns! Você criou uma cópia do repositório do laboratório e iniciou seu codespace, que usará junto ao agente de nuvem do Copilot.

## Próximo passo

Adicione instruções personalizadas que o agente de nuvem seguirá. Continue para o [Exercício 1 - Instruções personalizadas][next-lesson].

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[next-lesson]: ../1-custom-instructions/
