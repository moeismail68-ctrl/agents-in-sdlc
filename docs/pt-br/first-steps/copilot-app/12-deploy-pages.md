---
title: "Lição 12 - Implantar o site com GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar sua pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz uma presença pública implantando-o com GitHub Pages. O Copilot pode preparar o fluxo de trabalho e orientar a implantação, mas a responsabilidade pela revisão das alterações e pela confirmação de que o site está disponível continua sendo sua.

Nesta lição, você vai:

- pedir ao Copilot que configure uma implantação do GitHub Pages.
- revisar o fluxo de trabalho e a pull request antes da fusão.
- acompanhar a implantação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório GitHub, mas as pessoas precisam de uma maneira simples de experimentá-lo. Publicar o site de um único arquivo com GitHub Pages o torna disponível em uma URL pública, sem adicionar um servidor ou uma dependência de tempo de execução.

## Peça ao Copilot que prepare a implantação

> [!WARNING]
> Criar uma pull request pode confirmar e enviar código. Revise as configurações de aprovação e o repositório de destino antes de enviar este prompt. Com **Approve all**, não espere outra solicitação de confirmação.

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Prepare uma implantação do GitHub Pages para este repositório. Crie um fluxo de trabalho do GitHub Actions que compile o site, se necessário, e implante-o no GitHub Pages após as alterações chegarem ao branch padrão. Adicione um README descrevendo o aplicativo e como executá-lo localmente, caso ainda não exista. Crie uma pull request com essas alterações e informe o link e as configurações ou permissões necessárias do Pages. Não faça fusão, não habilite fusão automática, não altere as configurações do Pages e não implante. Pare e aguarde minha revisão.
```

## Revise o fluxo de trabalho e a pull request

1. Confirme que o README será adicionado somente se o repositório ainda não tiver um.
2. Revise as alterações da pull request, incluindo as etapas de compilação e implantação do fluxo de trabalho, os gatilhos e as permissões.
3. Confirme que o fluxo de trabalho faz a implantação a partir do branch padrão, não de uma pull request não mesclada.
4. Solicite correções e revise o diff atualizado. Aguarde a aprovação das verificações necessárias da pull request antes de autorizar uma fusão.

## Autorize a fusão e a implantação

> [!WARNING]
> O prompt a seguir autoriza o agente a mesclar a pull request revisada, alterar as configurações do Pages do repositório e publicar o site. Envie-o somente após concluir a revisão e confirmar o repositório e a pull request. As configurações de aprovação podem permitir essas ações sem outra solicitação de confirmação.

Substitua `<PR_NUMBER>` pelo número da pull request que você revisou e envie este prompt separado:

```plaintext
Revisei a pull request <PR_NUMBER> neste repositório e autorizo sua fusão e implantação. Habilite o GitHub Pages usando GitHub Actions, mescle essa pull request somente depois que as verificações necessárias forem aprovadas e acompanhe a implantação até que ela seja concluída. Informe a URL final do site publicado. Se você não tiver permissão para habilitar o Pages ou mesclar, informe exatamente o que eu preciso aprovar ou fazer; não informe sucesso até que o site esteja publicado.
```

![Diagrama em três etapas: uma pull request mesclada do space-quiz; uma execução bem-sucedida de Deploy website do GitHub Actions com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verifique a implantação

1. Acompanhe a execução de implantação do GitHub Actions até que ela seja concluída.
2. Abra a URL informada e confirme que o Space Quiz seja carregado.

Se o agente não conseguir habilitar o Pages ou mesclar a pull request, siga as instruções específicas fornecidas. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site publicado seja carregado.

## Resumo e próximos passos

Seu Space Quiz agora está disponível como um site publicado, e você verificou a implantação em vez de confiar no relatório do agente. Continue para a [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
---
