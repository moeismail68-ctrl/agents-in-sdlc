---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] permitem criar assistentes especializados para tarefas ou áreas específicas do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown dentro da pasta `.github/agents`, você fornece ao Copilot instruções, práticas recomendadas e padrões específicos que orientam tipos particulares de trabalho.

Os agentes podem ser definidos no repositório em `.github/agents` ou globalmente em `~/.copilot/agents`. Cada arquivo usa frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento e as instruções do agente.

### Agentes personalizados x skills de agente

Os agentes personalizados e as [skills de agente][agent-skills-concept] se complementam. Um agente é o trabalhador que coordena a execução; uma skill fornece uma capacidade reutilizável.

Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade.

> [!NOTE]
> Não existe uma única forma correta de criar um agente personalizado. Teste e itere conforme os ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills

## Cenário

O Tailspin Toys quer que sua plataforma seja acessível a todas as pessoas. O feedback recente indica que o tema escuro atual pode não ter contraste suficiente. Para melhorar a acessibilidade, você criará um modo de alto contraste que possa ser ativado pelos usuários.

## Revise o agente personalizado de acessibilidade

1. Retorne ao codespace e abra `.github/agents/accessibility.md`.
2. Examine o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios.

3. Revise as responsabilidades principais, as práticas recomendadas, os exemplos de código e os erros comuns descritos no agente.

## Crie e atribua uma issue

1. Acesse seu repositório.
2. Selecione a guia **Issues** e depois **New issue**.
3. Selecione **Blank issue**.
4. Defina o **Title** como `Add high contrast mode to website`.
5. Defina o **Description** como:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

6. Selecione **Create**.
7. Na barra lateral, selecione **Assign to Copilot**.
8. Selecione **Accessibility agent** na lista de agentes personalizados.
9. Selecione **Assign**.

## Resumo

Você explorou agentes personalizados e atribuiu uma tarefa especializada ao agente de nuvem.

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
