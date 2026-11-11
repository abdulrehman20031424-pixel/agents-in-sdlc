---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é fundamental ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de uma maneira específica — ou se há informações de contexto que o Copilot deve conhecer — você precisa garantir que esse contexto esteja acessível. Há várias maneiras de compartilhar contexto específico com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que fornecem orientação sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções associados a caminhos,
- gerar a primeira fatia de dados para filtragem (um auxiliar de editoras) com as instruções *atuais* em vigor,
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`,
- enviar o mesmo prompt novamente e observar o código gerado adotar o novo padrão,
- fazer commit das atualizações das instruções e da fatia de filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões que você definiu. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança no comportamento após a atualização das instruções, não obter um resultado idêntico em cada caractere.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- A documentação deve ser adicionada ao código na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever a finalidade do arquivo.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de código e seus requisitos. Esse recurso permite orientar as sugestões e os trechos de código para que sejam mais relevantes. Você pode especificar convenções de código, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos, oferecendo contexto direcionado à tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo de instruções enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações feitas por chat ou pelo CLI.
- Arquivos `.github/instructions/*.instructions.md`, que fornecem diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instruções varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explore os arquivos de instruções personalizadas deste projeto

O projeto inicial já inclui um conjunto de arquivos de instruções. Vamos explorá-los antes de fazer alterações para observar o impacto delas.

1. Volte ao seu Codespace.
2. No editor do Codespaces (não no terminal), abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e as orientações de código. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Observe que há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob que determina os arquivos aos quais as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir.

## Atualize as instruções personalizadas antes de usar o Copilot Chat

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao seu codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção de modelo variam em cada exercício — cada etapa indicará o que é necessário.

Para ver o impacto das instruções personalizadas, primeiro envie um prompt com o arquivo de instruções atual. Depois, você o atualizará e enviará o mesmo prompt novamente para comparar os resultados.

1. Feche as guias de editor abertas de exercícios anteriores para que o Copilot receba somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. Selecione **Agent** no seletor de agentes da visualização Chat para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o seguinte prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará atualizações de código, muitas vezes em vários arquivos, incluindo o auxiliar e seus testes.
6. Observe que o auxiliar proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras — isso vem das convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicável a `src/lib/*.ts`.
7. Observe que o código proposto **não tem** comentários TSDoc nem cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, ele pode adicionar comentários de documentação mesmo sem essa orientação. Se isso acontecer, tudo bem — o objetivo é observar a melhora da *consistência* depois da atualização das instruções.

## Adicione um novo padrão para o repositório

Como mencionado, `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Vamos documentar os padrões de código do repositório para aprimorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**. Observe como ela documenta os padrões de código do projeto, mas ainda não tem regras para documentação no código — por isso, o auxiliar gerado não tinha comentários de documentação.
3. Adicione as linhas de Markdown a seguir logo abaixo dos padrões existentes, para orientar o Copilot a adicionar cabeçalhos de comentário nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para fornecer orientações gerais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões que abrangem o projeto, como a regra dos comentários de documentação.

## Execute o prompt novamente e observe a diferença

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o mesmo prompt de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que agora o arquivo proposto começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Auxiliares de acesso a dados de editoras da plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que agora a função proposta inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com seus IDs e nomes.
    *
    * @param db - Cliente de banco de dados Drizzle.
    * @returns Uma promessa resolvida com uma matriz de objetos de editoras.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo a código real que servirá de base para o próximo exercício.

## Faça commit das instruções e envie a branch

Os arquivos de instruções são como qualquer outro recurso do repositório e são gerenciados pelo mesmo controle de versão. Vamos fazer commit das alterações e enviar a branch para o repositório.

1. Abra uma nova janela de terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. No terminal, confirme que as atualizações das instruções e do auxiliar estão presentes executando:

   ```bash
   git status
   ```

3. Prepare e faça commit das atualizações das instruções e de todos os arquivos que o Copilot alterou para criar a base do auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras atualizações de suporte (por exemplo, testes), adicione também esses arquivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle um pull request

Com a branch enviada, vamos criar um pull request e associá-lo à issue do backlog sobre o padrão de documentação. Poderíamos fazer isso manualmente, mas o Copilot pode encontrar a issue, criar um PR para fechá-la e depois mesclá-lo.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub por você. O template do projeto já conecta as ferramentas do GitHub ao seu workspace, então não é preciso configurar nada. Na primeira vez que o Copilot usar uma ferramenta, o VS Code poderá solicitar que você entre no GitHub. Siga as instruções para autorizá-lo. Você aprenderá como essa conexão funciona (Model Context Protocol) em um exercício futuro.

1. Abra o Copilot Chat no seu codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização do Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot para encontrar a issue sobre a atualização dos arquivos de instruções e criar um PR a partir da branch atual, descrevendo as atualizações das instruções e a nova base do auxiliar de editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a procurar a issue e criar o PR.
5. Quando solicitado a **Allow** que o Copilot execute ações no GitHub em seu nome, revise o comando e selecione **Allow** conforme apropriado.
6. Depois que o PR for criado, peça ao Copilot para mesclá-lo e voltar à branch `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado a **Allow** que o Copilot execute ações no GitHub e comandos do shell em seu nome, revise o comando e selecione **Allow** conforme apropriado.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*,
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`,
- enviar o mesmo prompt novamente e observar o código proposto adotar o novo padrão,
- fazer commit das atualizações das instruções e da base do auxiliar em `main` para que o próximo exercício possa usá-las.

Na próxima lição, você colocará as instruções em prática no [modo de agente][next-lesson] enquanto o Copilot adiciona uma nova funcionalidade à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
