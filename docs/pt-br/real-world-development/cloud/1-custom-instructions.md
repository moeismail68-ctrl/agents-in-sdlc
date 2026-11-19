---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Quando uma tarefa precisa ser realizada de determinada maneira — ou quando há informações sobre o projeto que o Copilot deve conhecer — esse contexto precisa estar acessível. Os [arquivos de instruções][instruction-files] fornecem essa orientação, para que o Copilot entenda não apenas **o que** você deseja fazer, mas **como** deseja fazê-lo.

Neste exercício, você vai:

- explorar como contexto específico do projeto, diretrizes de programação e padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções com escopo de caminho;
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Ao contrário dos workshops da CLI e do VS Code, você não executará prompts de antes/depois aqui. O agente de nuvem do Copilot funciona de forma assíncrona no GitHub, por isso o impacto é mais difícil de demonstrar em tempo real. A influência do arquivo de instruções aparecerá depois, quando você revisar as pull requests produzidas pelo agente de nuvem.

## Arquivos de instruções

### Cenário

Como em toda boa equipe de desenvolvimento, o Tailspin Toys tem um conjunto de diretrizes e requisitos para as práticas de desenvolvimento. Eles incluem:

- A camada de dados sempre precisa de testes de unidade.
- A interface deve usar modo escuro e ter uma aparência moderna.
- A documentação deve ser adicionada ao código como comentários TSDoc.
- Um bloco de comentários deve ser adicionado ao início de cada arquivo descrevendo sua finalidade.

Os arquivos de instruções garantirão que o Copilot tenha as informações corretas para executar as tarefas de acordo com essas práticas.

## Explore os arquivos de instruções existentes

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição breve do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**.
3. Abra a pasta `.github/instructions` e examine os arquivos disponíveis para tarefas como UI, Astro, testes e camada de dados.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`, que limita as instruções aos tipos de arquivo correspondentes.

## Adicione um novo padrão ao repositório

Adicione uma regra global segundo a qual a documentação deve ficar no código como comentários TSDoc e um cabeçalho de comentário no nível do arquivo:

1. Crie e alterne para um novo branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abra `.github/copilot-instructions.md`.
3. Localize a seção **Code formatting requirements**.
4. Adicione imediatamente abaixo das regras existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Salve o arquivo.

> [!NOTE]
> O texto dentro do bloco de código é uma configuração técnica e permanece em inglês; não é necessário traduzir palavras reservadas ou instruções literais do repositório.

## Confirme, envie e mescle a atualização

1. Prepare e confirme a alteração:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie o branch e abra uma pull request.
3. Aguarde as verificações e mescle a alteração em `main`.

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot
