---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é fundamental ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de uma maneira específica, você precisa garantir que essa orientação esteja acessível ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *qual* código você quer, mas também *como* ele deve ser estruturado. Agora que você implementou a filtragem, explorará as instruções usadas pelo Copilot, adicionará um padrão de documentação e o aplicará ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções associados a caminhos chegam ao agente.
- atualizar o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- Os comentários devem explicar a intenção e as decisões que não são óbvias, em vez de repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, os parâmetros e os valores de retorno com TSDoc/JSDoc, incluindo o argumento `db` injetável, quando presente.
- Os componentes Astro reutilizáveis devem documentar os contratos de `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações de lint e formatação existentes devem ser preservadas.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de código e seus requisitos. Esse recurso permite orientar as sugestões e os trechos de código para que sejam mais relevantes. Você pode especificar convenções de código, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos, oferecendo contexto direcionado à tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo de instruções enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações.
- Arquivos `.github/instructions/*.instructions.md`, que fornecem diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instruções varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explore os arquivos de instruções personalizadas deste projeto

Para começar, o projeto inicial já inclui um conjunto de arquivos de instruções. Vamos explorá-los antes de fazer alterações para observar o impacto.

1. Volte ao seu Codespace.
2. No editor do Codespaces (não no terminal), abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e as orientações de código. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Observe que há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob que determina os arquivos aos quais as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir.

## Atualize os arquivos de instruções de acordo com as orientações da equipe

Os arquivos existentes são um bom começo, mas ainda falta uma regra. Vamos modificar `copilot-instructions.md` para garantir que os novos códigos TypeScript incluam comentários TSDoc.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar perto da linha 35.
2. Adicione o seguinte como o último item da lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Use as orientações atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa é iniciada. Depois de salvar as instruções atualizadas, feche e retome a conversa de filtragem para que o Copilot carregue as novas orientações sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para fechá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot para aplicar as orientações atualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e leia os arquivos TypeScript alterados. Observe os comentários TSDoc gerados e confirme que descrevem o código com precisão.

## Resumo e próximas etapas

Você explorou como o Copilot CLI obtém as instruções do repositório e aplicou um novo padrão ao seu recurso. Em particular, você:

- explorou como as instruções do repositório e os arquivos de instruções associados a caminhos chegam ao agente.
- atualizou o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observou o impacto dos arquivos de instruções no código.

Na próxima etapa, você [personalizará e executará a skill reutilizável `quality-checks`][next-lesson] para garantir que o lint e os testes sejam executados de forma consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
