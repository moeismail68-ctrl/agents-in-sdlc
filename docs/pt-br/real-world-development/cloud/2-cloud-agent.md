---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Muitas equipes precisam lidar com dívidas técnicas: problemas de segurança não resolvidos, código legado que precisa ser atualizado ou solicitações de recursos que ficaram no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode executar tarefas como atualização de código e implementação de funcionalidades de forma autônoma. Quando o agente conclui o trabalho, gera uma pull request em rascunho para revisão por uma pessoa desenvolvedora.

Neste exercício, você vai:

- personalizar o ambiente usado para gerar código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de issues com escopo claro;
- atribuir issues ao Copilot.

## Cenários

O Tailspin Toys deseja reduzir a dívida técnica. A equipe que criou a primeira versão do site deixou a documentação incompleta. Como primeiro passo, você solicitará comentários TSDoc para todas as funções exportadas.

A equipe de design também quer melhorar a descoberta de jogos. A página de detalhes de cada jogo deverá mostrar jogos relacionados da mesma categoria. Esta primeira implementação pode ser básica, pois servirá a testes de aceitação da experiência.

## Conheça o agente de nuvem

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) pode realizar tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Ele cria uma pull request em rascunho, prepara um ambiente e começa a trabalhar após a issue ser atribuída. Você pode acompanhar a sessão durante ou depois da execução e responder às solicitações de revisão.

## Personalize o ambiente

O agente de nuvem usa GitHub Actions. O fluxo `.github/workflows/copilot-setup-steps.yml` prepara o ambiente antes da execução e pode instalar Node.js, dependências, navegadores do Playwright e o banco de dados usado pelo projeto.

> [!NOTE]
> O agente de nuvem fornece apenas acesso de leitura ao repositório e grava somente no branch criado para a tarefa. Os workflows precisam de aprovação humana antes da execução.

## Atribua uma issue

1. Acesse seu repositório no github.com.
2. Abra a guia **Issues** e selecione **New issue**.
3. Escolha **Blank issue**.
4. Use `Code lacks documentation` como título e solicite comentários TSDoc, cabeçalhos de arquivo e documentação adequada.
5. Crie a issue e selecione **Assign to Copilot**.
6. Aguarde o agente criar uma pull request em rascunho e iniciar o trabalho.

## Resumo

Você conheceu o agente de nuvem, personalizou seu ambiente e atribuiu uma tarefa clara que será implementada de forma assíncrona.

[previous-lesson]: ../1-custom-instructions/
